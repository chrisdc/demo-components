<template functional>
  <div class="menu-item">
    <div class="image-wrapper">
      <picture>
        <source v-bind:srcset="props.item.image2" media="(min-width: 405px)">
        <img v-bind:src="props.item.image" v-bind:alt="props.item.name">
      </picture>
    </div>

    <div class="text-wrapper">
      <h2 class="item-title">
        {{ props.item.name }}
      </h2>
      <p>
        {{ props.item.details }}
      </p>
      <p class="price">
        {{ props.item.price }}
      </p>
    </div>
  </div>
</template>
