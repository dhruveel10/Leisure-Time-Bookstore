<script setup lang="ts">
import { reactive, ref } from "vue";
import useVuelidate from "@vuelidate/core";
import { helpers, maxLength, minLength, required } from "@vuelidate/validators";
import { useCartStore } from "@/stores/cart";
import { isCreditCard, isMobilePhone } from "@/validators";
import { useRouter } from "vue-router";
import CheckoutFieldError from "@/components/CheckoutFieldError.vue";
import { email } from "@vuelidate/validators";
import {asDollarsAndCents} from "@/utils"
import type { OrderDetails, ServerErrorResponse } from "@/types";
import{useOrderDetails} from "@/stores/orderDetails";

const router = useRouter();
const cartStore = useCartStore();
const cart = cartStore.cart;

const cartSubtotalFormatted = (cart.subtotal)/100.0;
const orderDetailsStore = useOrderDetails();

const months: string[] = [
  "January",
  "February",
  "March",
  "April",
  "May",
  "June",
  "July",
  "August",
  "September",
  "October",
  "November",
  "December",
];

const form = reactive({
  name: "",
  address: "",
  phone: "",
  email: "",
  ccNumber: "",
  ccExpiryMonth: new Date().getMonth() + 1,
  ccExpiryYear: new Date().getFullYear(),
  checkoutStatus: "",
});

const rules = {
  name: {
    required: helpers.withMessage("Please provide a name.", required),
    minLength: helpers.withMessage(
      "Name must have at least 4 letters.",
      minLength(4)
    ),
    maxLength: helpers.withMessage(
      "Name can have at most 45 letters.",
      maxLength(45)
    ),
  },
  //   TODO: Add more validations for these and other fields that need more validation.
  phone:{
    required: helpers.withMessage("Please provide a phone number.", required),
    isMobilePhone: helpers.withMessage("Please provide a valid phone number.", isMobilePhone),
  },

  address:{
    required: helpers.withMessage("Please provide a valid address.", required),
    minLength: helpers.withMessage(
      "Address must have at least 4 letters.",
      minLength(8),
    ),
    maxLength: helpers.withMessage(
      "Address can have at most 45 letters.",
      maxLength(45)
    ),
  },

  email: {
    required: helpers.withMessage("Please provide an email address.", required),
    email: helpers.withMessage("Please provide a valid email address.", email),
  },

  ccNumber:{
    required: helpers.withMessage("Please enter your credit card number.", required),
    isCreditCard: helpers.withMessage("Please provide a valid credit card number.", isCreditCard),
  },
};
const v$ = useVuelidate(rules, form);

const defaultServerErrorMessage = "An unexpected error occurred, please try again."
const serverErrorMessage = ref(defaultServerErrorMessage);

async function submitOrder() {
  console.log("Submit order");
  const isFormCorrect = await v$.value.$validate();
  if (!isFormCorrect) {
    form.checkoutStatus = "ERROR";
  } else {
    try {
      form.checkoutStatus = "PENDING";
      serverErrorMessage.value = defaultServerErrorMessage;

      const placeOrderResponse: OrderDetails | ServerErrorResponse =
        await cartStore
          .placeOrder({
            name: form.name,
            address: form.address,
            phone: form.phone,
            email: form.email,
            ccNumber: form.ccNumber,
            ccExpiryMonth: form.ccExpiryMonth,
            ccExpiryYear: form.ccExpiryYear,
          })

      if ("error" in placeOrderResponse) {
        form.checkoutStatus = "SERVER_ERROR";
        serverErrorMessage.value = placeOrderResponse.message
        console.log("Error placing order", placeOrderResponse);
      } else {
        form.checkoutStatus = "OK";
        orderDetailsStore.setOrderDetails(placeOrderResponse as OrderDetails);
        setTimeout(async () => {
          // Redirect to confirmation view after a short delay
          await router.push({ name: "confirmation-view" });
        }, 200);
      }

    } catch (e) {
      form.checkoutStatus = "SERVER_ERROR";
      serverErrorMessage.value = defaultServerErrorMessage;
      console.log("Error placing order", e);
    }
  }
}

/* NOTE: For example yearFrom(0) == <current_year> */
function yearFrom(index: number) {
  return new Date().getFullYear() + index;
}

function generateExpiryYears() {
  const currentYear = new Date().getFullYear();
  const years = [];
  for (let i = 0; i < 15; i++) {
    years.push(currentYear + i);
  }
  return years;
}
</script>

<style scoped>
/* TODO: Adapt these styles to your page */
.checkout-page {
  background: #dcd1c3;
  color: black;
  padding: 1em; /* Add some padding to the whole checkout page */
}

.form-details {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.form-details div {
  margin-bottom: 1em; 
}

.cart-summary div {
  margin-bottom: 1em; 
}

.checkout-page {
  background: #dcd1c3;
  color: black;
}
.checkout-page-body {
  display: flex;
  padding: 1em;
}

form {
  display: flex;
  flex-direction: column;
}

form > div {
  display: flex;
  justify-content: flex-end;
  margin-bottom: 0.5em;
}

form > div > input,
form > div > select {
  background-color: #ffffff;
  margin-left: 0.5em;
  border: none;
}

form > .error {
  color: red;
  text-align: right;
}

.checkoutStatusBox {
  margin: 1em;
  padding: 1em;
  text-align: center;
}

.expiry-label {
  display: inline-block;
  width: 90px; /* Adjust the width to your preference */
  text-align: right;
  margin-right: 15px; /* Adjust the margin to create space between the label and selects */
}

.expiry-selects {
  display: flex;
}

.expiry-selects select {
  margin-right: 10px; /* Adjust the margin to create space between the month and year selects */
}

.center-button {
  display: block;
  margin: 0 auto;
  width: 200px; /* Adjust the width as needed */
  padding: 10px;
  background-color: rgb(135, 102, 71);
}

.cart-summary {
  display: flex;
  flex-direction: column; /* Display items and subtotal vertically */
}
.cart-summary .items-info label,
.cart-summary .payment-info label {
  margin-right: 10px; /* You can adjust the value to control the spacing */
}


.items-info, .subtotal-info {
  display: flex;
  justify-content: space-between; /* Add space between label and value */
}

.total-amount {
  font-weight: bold;
  /* You can add more styles here as needed */
}

.checkout-status-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 50px; /* Adjust the height to your preference */
  color: red; /* Red color for the font */
  font-size: 24px; /* Adjust the font size as needed */
}


</style>

<template>
  <div class="checkout-page">
    <div class="checkout-status-container" style="margin-top: -10px;">
      <div v-if="cart.empty">
        <div>The Cart is empty</div>
      </div>
    </div>

    <div class="checkout-status-container" style="margin-top: -10px;">
    <div v-if="form.checkoutStatus === 'PENDING'">
      Pending...
    </div>
    <div class="error" v-else-if="form.checkoutStatus === 'ERROR' || form.checkoutStatus === 'SERVER_ERROR'">
      {{ serverErrorMessage }}
    </div>
    <div v-else-if="form.checkoutStatus === 'OK'">
      Order placed successfully!
    </div>
  </div>

    <section class="checkout-page-body" v-if="!cart.empty">
      <div class="form-details">
        
        <form @submit.prevent="submitOrder">
          <div>
            <label for="name">Name</label>
            <input
              type="text"
              size="20"
              id="name"
              name="name"
              v-model.lazy="v$.name.$model"
            />
        </div>
        <CheckoutFieldError :field-name="v$.name"/>


          <!-- TODO: Add address input and validation messages -->
          <div>
            <label for="address">Address</label>
            <input
              type="text"
              size="20"
              id="address"
              name="address"
              v-model.lazy="v$.address.$model"
            />
          </div>
          <CheckoutFieldError :field-name="v$.address"/>

          <div>
            <label for="phone">Phone</label>
            <input
              class="textField"
              type="text"
              size="20"
              id="phone"
              name="phone"
              v-model.lazy="v$.phone.$model"
            />
          </div>
          <!-- TODO: Add phone validation message(s) -->
          <CheckoutFieldError :field-name="v$.phone"/>

          <div>
            <label for="email">Email</label>
            <input type="text" size="20" id="email" name="email" v-model.lazy="v$.email.$model" />
          </div>
          <CheckoutFieldError :field-name="v$.email" />


          <div>
            <label for="ccNumber">Credit card</label>
            <input type="text" size="20" id="ccNumber" name="ccNumber" 
              v-model.lazy="v$.ccNumber.$model"
            />
          </div>
          <!-- TODO: Add credit card validation message(s) -->
          <CheckoutFieldError :field-name="v$.ccNumber"/>

          <!-- TODO (style): Use a single label for both month and date and put the on the same line. -->
          <!-- TODO (style): For example: Exp Date {Month} {Year}, with space between month/year selectors. -->

          <div>
            <label for="ccExpiry" class="expiry-label">Exp Date</label>
            <div class="expiry-selects">
              <select v-model="v$.ccExpiryMonth" required>
                <option
                  v-for="(month, index) in months"
                  :key="index"
                  :value="index + 1"
                >
                  {{ month }}
                </option>
              </select>
              <select v-model="v$.ccExpiryYear" required>
                <option v-for="year in generateExpiryYears()" :key="year" :value="year">{{ year }}</option>
              </select>
            </div>
          </div>
          
          <input
            type="submit"
            name="submit"
            class="button center-button"
            :disabled="form.checkoutStatus === 'PENDING'"
            value="Complete Purchase"
            style="width: 200px; padding: 10px; background-color: rgb(135, 102, 71);"
          />
        </form>
      </div>

      <div class="cart-summary">
        <h3>Order Summary</h3>
        <table>
          <hr>
          <tr>
            <td>Number of items in the cart:</td>
            <td>{{ cart.numberOfItems }}</td>
          </tr>
          <tr>
            <td>Order total:</td>
            <td>{{ asDollarsAndCents(cart.subtotal) }}</td>
          </tr>
          <tr>
            <td>Delivery Charges:</td>
            <td>$5</td>
          </tr>
          <tr>
            <td>Taxes:</td>
            <td>{{ asDollarsAndCents(cart.subtotal % 10) }}</td>
          </tr>
          <hr>
          <tr>
            <td><strong>Total:</strong></td>
            <td><strong>${{ cartSubtotalFormatted + 5 + cartSubtotalFormatted % 10 }}</strong></td>
          </tr>
        </table>
      </div>
      </section>

        <!-- TODO (style): The submit button should not take up the entire width of the form. -->
        <!-- TODO (style): The submit button should be styled consistent with your own site. -->
      <!-- TODO (style): Fix error message placement so they nearer to the correct fields. -->
      <!-- TODO (style): HINT: Use another <div> and label, input, and error, and use flexbox to style. -->

      <!-- TODO: Display the cart total, subtotal and surcharge. -->
  </div>
</template>
