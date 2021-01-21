<template>
  <section class="pricing-section spad">
    <ul class="box-area">
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
    </ul>

    <a role="button" id="cart" v-b-modal.modal-cart>
      <b-modal id="modal-cart" title="Shopping Cart" hide-footer>
        <h2 v-if="this.checkOutClicked">Your order has been submitted</h2>
        <table v-else class="table">
          <tbody>
            <tr>
              <td>Class</td>
              <td>Price</td>
              <td></td>
            </tr>
            <tr v-for="(lesson, index) in cart" :key="index">
              <td>{{ lesson.name }}</td>
              <td>£{{ lesson.price }}</td>
              <td>
                <a role="button" v-on:click="remove(index)"
                  ><img
                    src="https://upload.wikimedia.org/wikipedia/en/thumb/b/ba/Red_x.svg/1200px-Red_x.svg.png"
                    width="30"
                /></a>
              </td>
            </tr>
            <tr>
              <td><strong style="float: right">Total:</strong></td>
              <td>£{{ total }}</td>
              <td></td>
            </tr>
          </tbody>
        </table>
        <h4 v-if="this.checkOutClicked">
          Thank you for shopping with us 🤩
          <hr />
          Refresh page to make another order 👨‍🏫
        </h4>

        <form v-else id="form">
          <b-form-group label="Name" label-for="name-input">
            <b-form-input
              name="name"
              id="name-input"
              placeholder="Enter your full name"
              v-model="name"
              @input="validateData()"
            ></b-form-input>
          </b-form-group>

          <b-form-group label="Telephone" label-for="telephone-input">
            <b-form-input
              id="telephone-input"
              placeholder="Enter your phone number"
              v-model="telephone"
              @input="validateData()"
            ></b-form-input>
          </b-form-group>

          <b-button
            v-if="this.validation && this.cart.length > 0"
            class="mt-3"
            variant="outline-primary"
            @click="checkout"
            >Check Out</b-button
          >
          <b-button v-else variant="secondary" disabled
            >Enter valid name (letters only) and valid UK telephone number above
            😉</b-button
          >
        </form>
      </b-modal>
      <img
        src="https://icons.iconarchive.com/icons/google/noto-emoji-objects/1024/63007-shopping-cart-icon.png"
        v-if="cart.length > 0"
        width="40"
      />
      <span v-if="cart.length > 0" class="badge badge-warning" id="cartCount">
        {{ cart.length }}
      </span>
    </a>
    <div class="container">
      <b-form-input
        type="search"
        placeholder="Search"
        aria-label="Search"
        name="search"
        id="searchInput"
        v-model="searchQuery"
        @change="displayClasses()"
      ></b-form-input>

      <div>
        <b-form-select
          v-model="selected"
          :options="options"
          id="sortBy"
          class="mt-3"
          @change="sortClasses()"
        ></b-form-select>
      </div>

      <div class="container" id="classes">
        <div class="row">
          <div
            v-for="(activity, index) in displayClasses"
            :key="index"
            class="col-lg-4 col-md-6 col-sm-12"
          >
            <ul>
              <li
                class="booking-card"
                :style="{ backgroundImage: `url('${activity.imageURL}')` }"
              >
                <div class="book-container">
                  <div class="content">
                    <button
                      v-if="activity.availableSpaces > 0"
                      class="btn"
                      :id="index"
                      v-on:click="reduce(index)"
                    >
                      Book Me
                    </button>
                    <button
                      v-else
                      class="btn"
                      :id="index"
                      v-on:click="reduce(index)"
                      disabled
                    >
                      Not Available
                    </button>
                  </div>
                </div>
                <div class="informations-container">
                  <h2 class="title">{{ activity.topic }}</h2>
                  <p class="sub-title">{{ activity.user }}</p>
                  <p class="price">
                    <svg
                      class="icon"
                      style="width: 24px; height: 24px"
                      viewBox="0 0 24 24"
                    >
                      <path
                        fill="currentColor"
                        d="M3,6H21V18H3V6M12,9A3,3 0 0,1 15,12A3,3 0 0,1 12,15A3,3 0 0,1 9,12A3,3 0 0,1 12,9M7,8A2,2 0 0,1 5,10V14A2,2 0 0,1 7,16H17A2,2 0 0,1 19,14V10A2,2 0 0,1 17,8H7Z"
                      /></svg
                    >£ {{ activity.price }}/month
                  </p>
                  <div class="more-information">
                    <p class="disclaimer">
                      Available Spaces: {{ activity.availableSpaces }}
                    </p>
                    <div class="info-and-date-container">
                      <div class="box info">
                        <svg
                          class="icon"
                          style="width: 24px; height: 24px"
                          viewBox="0 0 24 24"
                        >
                          <path
                            fill="currentColor"
                            d="M11,9H13V7H11M12,20C7.59,20 4,16.41 4,12C4,7.59 7.59,4 12,4C16.41,4 20,7.59 20,12C20,16.41 16.41,20 12,20M12,2A10,10 0 0,0 2,12A10,10 0 0,0 12,22A10,10 0 0,0 22,12A10,10 0 0,0 12,2M11,17H13V11H11V17Z"
                          />
                        </svg>
                        <p>{{ activity.location }}</p>
                      </div>
                      <div class="box date">
                        <svg
                          class="icon"
                          style="width: 24px; height: 24px"
                          viewBox="0 0 24 24"
                        >
                          <path
                            fill="currentColor"
                            d="M19,19H5V8H19M16,1V3H8V1H6V3H5C3.89,3 3,3.89 3,5V19A2,2 0 0,0 5,21H19A2,2 0 0,0 21,19V5C21,3.89 20.1,3 19,3H18V1M17,12H12V17H17V12Z"
                          />
                        </svg>
                        <p>{{ activity.date }}</p>
                      </div>
                    </div>
                  </div>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import $ from "jquery";

export default {
  name: "Classes",
  data() {
    return {
      classes: [],
      searchQuery: "",
      selected: null,
      options: [
        { value: null, text: "Sort by" },
        { value: "1", text: "Topic: A-Z" },
        { value: "2", text: "Topic Z-A" },
        { value: "3", text: "Location: A-Z" },
        { value: "4", text: "Location: Z-A" },
        { value: "5", text: "Price: Low to High" },
        { value: "6", text: "Price: High to Low" },
        { value: "7", text: "Availability: Low to High" },
        { value: "8", text: "Availability: High to Low" },
      ],
      cart: [],
      name: "",
      telephone: "",
      validation: false,
      checkOutClicked: false,
    };
  },
  created: function () {
    let vue = this;
    fetch("http://localhost:3000/collection/lessons").then((response) => {
      response
        .json()
        .then(function (json) {
          vue.classes = json;
        })
        .catch((error) => {
          alert("Error! " + error.message);
        });
    });
  },
  methods: {
    reduce(index) {
      this.classes[index].availableSpaces--;
      this.cart.push({
        name: this.classes[index].topic,
        price: this.classes[index].price,
        img: this.classes[index].imageURL,
      });
      if (this.classes[index].availableSpaces < 1) {
        document.getElementById(index).disabled = true;
        document.getElementById(index).innerHTML = "Not Available";
      }
    },
    remove(index) {
      let j = this.classes
        .map(function (e) {
          return e.topic;
        })
        .indexOf(this.cart[index].name);
      this.cart.splice(index, 1);
      this.classes[j].availableSpaces++;
      if (this.classes[j].availableSpaces > 0) {
        document.getElementById(j).disabled = false;
        document.getElementById(j).innerHTML = "Book Me";
      }
    },
    sortClasses() {
      if (this.selected == 1) {
        return this.classes.sort((a, b) => a.topic.localeCompare(b.topic));
      } else if (this.selected == 2) {
        return this.classes.sort((a, b) => b.topic.localeCompare(a.topic));
      } else if (this.selected == 3) {
        return this.classes.sort((a, b) =>
          a.location.localeCompare(b.location)
        );
      } else if (this.selected == 4) {
        return this.classes.sort((a, b) =>
          b.location.localeCompare(a.location)
        );
      } else if (this.selected == 5) {
        return this.classes.sort((a, b) => a.price - b.price);
      } else if (this.selected == 6) {
        return this.classes.sort((a, b) => b.price - a.price);
      } else if (this.selected == 7) {
        return this.classes.sort(
          (a, b) => a.availableSpaces - b.availableSpaces
        );
      } else if (this.selected == 8) {
        return this.classes.sort(
          (a, b) => b.availableSpaces - a.availableSpaces
        );
      }
    },
    showModal() {
      let element = this.$refs.modal.$el;
      $(element).modal("show");
    },
    validateData() {
      let lettersRegex = /^[a-zA-Z\s]*$/;
      let telephoneRegex = /^(((\+44\s?\d{4}|\(?0\d{4}\)?)\s?\d{3}\s?\d{3})|((\+44\s?\d{3}|\(?0\d{3}\)?)\s?\d{3}\s?\d{4})|((\+44\s?\d{2}|\(?0\d{2}\)?)\s?\d{4}\s?\d{4}))(\s?(\d{4}|\d{3}))?$/;
      if (
        !this.name.match(lettersRegex) ||
        this.name == "" ||
        !this.telephone.match(telephoneRegex) ||
        this.telephone == ""
      ) {
        this.validation = false;
      } else {
        this.validation = true;
      }
    },
    checkout() {
      this.checkOutClicked = true;
      let order = {
        name: this.name,
        telephone: this.telephone,
        lessons: [],
        total: 0,
      };

      this.cart.forEach((lesson) => {
        order.total = order.total + lesson.price;

        order.lessons.push({
          name: lesson.name,
          price: lesson.price,
        });

        this.classes.forEach((product) => {
          if (lesson.name == product.topic) {
            let updatedLesson = {
              availableSpaces: product.availableSpaces,
            };

            fetch(`http://localhost:3000/collection/lessons/${product._id}`, {
              method: "PUT", // set the HTTP method as 'PUT'
              headers: {
                "Content-Type": "application/json", // set the data type as JSON
              },
              body: JSON.stringify(updatedLesson), // need to stringify the JSON object
            })
              .then((response) => response.json())
              .then((responseJSON) => {
                console.log("Success:", responseJSON);
              });
          }
        });
      });

      // set the url to your server and route
      fetch("http://localhost:3000/collection/orders", {
        method: "POST", // set the HTTP method as 'POST'
        headers: {
          "Content-Type": "application/json", // set the data type as JSON
        },
        body: JSON.stringify(order), // need to stringify the JSON object
      })
        .then((response) => response.json())
        .then((responseJSON) => {
          console.log("Success:", responseJSON);
        });
      this.cart = [];
    },
  },
  computed: {
    displayClasses() {
      if (this.searchQuery) {
        return this.classes.filter((item) => {
          return this.searchQuery
            .split(" ")
            .every((search) => item.topic.toLowerCase().includes(search));
        });
      } else {
        return this.classes;
      }
    },
    total() {
      return this.cart.reduce((acc, item) => acc + item.price, 0);
    },
  },
};
</script>

<style scoped>
#searchForm {
  padding: 20px;
  text-align: center;
}

.pricing-section {
  text-align: center;
  min-height: 100%;
  padding: 20px;
  background: -webkit-gradient(
    linear,
    right top,
    left top,
    from(#8942a8),
    to(#ba382f)
  );
}

#cartCount {
  position: absolute;
  margin-right: 20px;
  z-index: 3;
}

#cart {
  position: absolute;
  right: 2%;
  top: 1%;
}
</style>