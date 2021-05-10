<template>
  <v-app>
    <v-content>
      <div
        style="display:flex;height:100%;background-color:black;padding:0 !important;margin:0 !important;"
      >
        <v-card color="white" max-width="372" style="margin:auto;min-height:700px;" class="mx-auto">
          <v-list-item>
            <v-avatar tile>
              <img style="height:24px;width:auto;" alt="Avatar" src="@/assets/logo.png" />
            </v-avatar>

            <v-list-item-content>
              <v-list-item-title class="headline">{{sneakers[activeIndex].name}}</v-list-item-title>
              <v-list-item-subtitle>by {{sneakers[activeIndex].designer}}</v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>

          <v-img :src="sneakers[activeIndex].img" height="250">
            <template v-slot:placeholder>
              <v-row class="fill-height ma-0" align="center" justify="center">
                <v-progress-circular indeterminate color="red"></v-progress-circular>
              </v-row>
            </template>
          </v-img>

          <v-card-actions>
            <v-btn @click="removeFromCart(sneakers[activeIndex])" icon color="black">
              <v-icon>mdi-minus</v-icon>
            </v-btn>
            <v-btn @click="addToCart(sneakers[activeIndex])" icon color="black">
              <v-icon small>mdi-plus</v-icon>
            </v-btn>
            <v-spacer></v-spacer>
            <v-btn @click="previous()" icon small>
              <v-icon>mdi-chevron-left</v-icon>
            </v-btn>
            <v-btn @click="next()" icon>
              <v-icon>mdi-chevron-right</v-icon>
            </v-btn>
          </v-card-actions>

          <div style="display:flex; margin-top:50px;">
            <!-- <v-btn large style="margin:auto;" color="black white--text">PAY NGN 1000</v-btn> <br> -->
            <paystack
              style="margin:auto;"
              class="v-btn v-btn--contained theme--light v-size--large black white--text"
              :amount="amount * 100"
              :email="user.email"
              :paystackkey="PUBLIC_KEY"
              :callback="processPayment"
              :reference="genRef()"
              :close="close"
              :embed="false"
            >PAY NGN {{amount}}</paystack>
          </div>
        </v-card>
        <v-snackbar multi-line v-model="snackbar">
          {{ snacktext }}
          <v-btn color="pink" text @click="snackbar = false">Close</v-btn>
        </v-snackbar>
      </div>
    </v-content>
  </v-app>
</template>

<script>
import paystack from "vue-paystack";
import uniqid from "uniqid";
import img1 from "@/assets/1.png";
import img2 from "@/assets/3.png";
import img3 from "@/assets/2.png";
import img4 from "@/assets/4.png";
export default {
  name: "App",

  components: {
    paystack
  },
  methods: {
    close() {},
    genRef() {
      return uniqid("pstk-");
    },
    processPayment() {
      this.cart = [];
      this.snackbar = true;
      this.snacktext = "Payment Successful! Your sneakers are on their way.";
    },
    next() {
      if (this.activeIndex == this.sneakers.length - 1) {
        this.activeIndex = 0;
      } else {
        this.activeIndex++;
      }
    },
    removeFromCart() {
      this.cart.splice(-1, 1);
    },
    addToCart() {
      this.cart.push(this.sneakers[this.activeIndex]);
    },
    previous() {
      if (this.activeIndex == 0) {
        this.activeIndex = this.sneakers.length - 1;
      } else {
        this.activeIndex--;
      }
    }
  },
  computed: {
    amount() {
      if (this.cart.length === 0) {
        return 0;
      }
      let total = 0;
      this.cart.forEach(i => {
        total += i.amount;
      });
      return total;
    }
  },
  data: () => ({
    snackbar: false,
    snacktext: "",
    user: {
      name: "John Doe",
      email: "customer@email.com"
    },
    PUBLIC_KEY: "pk_test_xxxxx",
    cart: [],
    sneakers: [
      {
        name: "Kobe IX",
        img: img1,
        designer: "Troy Nadir",
        amount: 37000
      },
      {
        name: "Twill Colorways",
        img: img2,
        designer: "Kurt Reynolds",
        amount: 20000
      },
      {
        name: "Air Splash",
        img: img3,
        designer: "Annie Shelby",
        amount: 12000
      },
      {
        name: "Jordan XII",
        img: img4,
        designer: "Annie Shelby",
        amount: 23000
      }
    ],
    activeIndex: 0
  })
};
</script>
