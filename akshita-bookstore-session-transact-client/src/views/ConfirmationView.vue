<script setup lang="ts">
    import ConfirmationTable from "@/components/ConfirmationTable.vue";
    import { asDollarsAndCents } from "@/utils";
    import { useOrderDetails} from "@/stores/orderDetails";
    import { computed } from "vue";
    import type { BookItem } from "@/types";
    import { useCartStore } from "@/stores/cart";
    import { useCategoryStore } from "@/stores/category";
    const cartStore = useCartStore();
    const categoryStore = useCategoryStore();
    const cart = cartStore.cart
    const orderDetailsStore = useOrderDetails();
    const orderDetails = orderDetailsStore.orderDetails;
    ;

    function bookImageUrl(imageFileName: string) {
    return new URL(`../assets/book-images/${imageFileName}`, import.meta.url).href
    }

    const orderDate = computed(function () {
    let date = new Date(orderDetails.order.dateCreated);
    return date.toLocaleDateString() +' '+ date.toLocaleTimeString();
    });

    const bookImageFileName = function (book: BookItem): string {
    let name = book.title.toLowerCase();
    name = name.replace(/ /g, "-");
    name = name.replace(/'/g, "");
    return `${name}.gif`;
    };

</script>

<style scoped>

p{
  line-height: 1.5;
    padding-right: 1em;
    text-align: right;
}
.button-area {
  display: flex;
  align-content: center;
  justify-content: center;
}

#confirmationView {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    margin: 1em auto;
  }

  .text-details {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
  }

  .left-details {
    margin-left: 5rem;
    margin-right: 10rem; /* Adjust spacing */
  }

  .right-details {
    margin-left: 10rem; /* Adjust spacing */
  }

ul {
  text-align: left;
  background-color: #dcd1c3;
  border-radius: 1rem;
  width: 20rem;
}

ul > li {
  margin: 0.25em;
  text-align: center;
  padding:0.1rem;
}

.space-maker{
    flex-grow: 5;
    display: flex;
    flex-direction: column;
    flex-basis: 5;
}

h1{
  font-size: 1.5rem;
  text-align: center;
  margin-bottom: 1rem;
  margin-top:1rem;
}

.call-to-action {
  box-shadow: 0px 4px 5px rgba(0, 0, 0, 0.5);
  border-radius: 1rem;
  border:0;
  background-color: var(--default-text-color);
  padding: 1rem;
  cursor: pointer;
  margin-top: 1rem;
}
.call-to-action:hover {
  background-color: var(--highlighted-color);
}

#confirmation-invalid{
  display:flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  text-align: center;
  padding:1rem;
}

.invalid-text{
  font-size: 1.5rem;
}

img {
    height: 20rem;
    width: 20rem;
    align-self: center;
}

</style>

<template>
    <div id="confirmationView">
      <div class="text-details left-details">
        <div id="confirmationDetails">
          <h1>CONFIRMATION DETAILS:</h1>
          <ul>
            <li>Confirmation Number: {{ orderDetails.order.confirmationNumber }}</li>
            <li>Time: {{ orderDate }}</li>
            <li>Total: {{ asDollarsAndCents(orderDetails.order.amount) }}</li>
            <li>Credit Card: **** **** **** {{
              orderDetails?.customer?.ccNumber.slice(
                orderDetails?.customer?.ccNumber?.length - 4
              )
            }}</li>
          </ul>
        </div>
      </div>
    
      <div class="text-details right-details">
        <div id="customerInfo">
            <h1>CUSTOMER DETAILS:</h1>
            <ul>
              <li>Name: {{ orderDetails.customer.name }}</li>
              <li>Address: {{ orderDetails.customer.address }}</li>
              <li>Email: {{ orderDetails.customer.email }}</li>
              <li>Phone: {{ orderDetails.customer.phone }}</li>
            </ul>
        </div>
      </div>
    </div>
  <!--
    <div class="order-details">
     
        <h1>ORDER DETAILS:</h1>
        <confirmation-table></confirmation-table>
     
    </div>
    -->
  </template>
