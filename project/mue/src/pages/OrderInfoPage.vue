<template>
  <main class="content container" v-if="isLoading">
    <div class="loader-wrapper">
      <base-loader></base-loader>
    </div>
  </main>
  <main class="content container" v-else-if="isLoadingFailed">
    <h1>Произошла ошибка при загрузке корзины. Попробуйте ещё раз!</h1>
  </main>
  <main class="content container" v-else>
    <div class="content__top">
      <ul class="breadcrumbs">
        <li class="breadcrumbs__item">
          <router-link class="breadcrumbs__link" :to="{ name: 'main' }">
            Каталог
          </router-link>
        </li>
        <li class="breadcrumbs__item">
          <a class="breadcrumbs__link" :to="{ name: 'cart' }">
            Корзина
          </a>
        </li>
        <li class="breadcrumbs__item">
          <a class="breadcrumbs__link">
            Оформление заказа
          </a>
        </li>
      </ul>

      <h1 class="content__title">
        Заказ оформлен <span>№ {{ orderInfo.id }}</span>
      </h1>
    </div>

    <section class="cart">
      <form class="cart__form form" action="#" method="POST">
        <div class="cart__field">
          <p class="cart__message">
            Благодарим за&nbsp;выбор нашего магазина. На&nbsp;Вашу почту придет письмо с&nbsp;деталями заказа. 
            Наши менеджеры свяжутся с&nbsp;Вами в&nbsp;течение часа для уточнения деталей доставки.
          </p>

          <ul class="dictionary">
            <li class="dictionary__item">
              <span class="dictionary__key">
                Получатель
              </span>
              <span class="dictionary__value">
                {{ orderInfo.name }}
              </span>
            </li>
            <li class="dictionary__item">
              <span class="dictionary__key">
                Адрес доставки
              </span>
              <span class="dictionary__value">
                {{ orderInfo.address }}
              </span>
            </li>
            <li class="dictionary__item">
              <span class="dictionary__key">
                Телефон
              </span>
              <span class="dictionary__value">
                {{ orderInfo.phone }}
              </span>
            </li>
            <li class="dictionary__item">
              <span class="dictionary__key">
                Email
              </span>
              <span class="dictionary__value">
                {{ orderInfo.email }}
              </span>
            </li>
            <li class="dictionary__item">
              <span class="dictionary__key">
                Способ оплаты
              </span>
              <span class="dictionary__value">
                {{ orderInfo.paymentType }}
              </span>
            </li>
          </ul>
        </div>

        <div class="cart__block">
          <ul class="cart__orders">
            <li class="cart__order" v-for="item in orderInfo.basket.items" :key="item.id">
              <h3>{{ item.product.title }}</h3>
              <b>{{ item.product.price }} ₽</b>
                <h3>
                Размер: {{ item.size.title }}
              </h3>
              <span>
                Артикул: {{ item.product.id }}
            </span>
            </li>
          </ul>
          
          <div class="cart__total">
            <p>Доставка: <b>{{ +orderInfo.deliveryType.price ? `${ orderInfo.deliveryType.price } ₽` : 'бесплатно' }}</b></p>
            <p>Итого: <b>{{ orderInfo.basket.items.length }}</b> товара на сумму <b>{{ orderInfo.totalPrice }} ₽</b></p>
          </div>
        </div>
      </form>
    </section>
  </main>
</template>

<script>
export default {
  computed: {
    orderInfo() {
      return this.$store.state.orderInfo;
    },
    isLoading() {
      return this.$store.state.isLoading;
    },
    isLoadingFailed() {
      return this.$store.state.isLoadingFailed;
    },
  },
  created() {
    this.$store.dispatch('loadOrderInfo', this.$route.params.id);
  }
}
</script>