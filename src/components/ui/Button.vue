<template>
  <div>
    <button class="btn5">
      <slot></slot>
    </button>
  </div>
</template>

<style scoped>
.btn5 {
    display: inline-block; 
    box-sizing: border-box;
    padding: 0 20px;
    margin: 0 15px 15px 0;
    outline: none;
    border: none; 
    border-radius: 6px;
    height: 40px;
    line-height: 40px;
    font-size: 17px;
    font-weight: 600;
    text-decoration: none;
    color: #385898;
    background-color: #e7f3ff;
    cursor: pointer;
    user-select: none;
    appearance: none;
    touch-action: manipulation;
}

.btn5:focus-visible {
    box-shadow: 0 0 0 2px #666;
}
.btn5:hover {
    background-color: #DBE7F2;
}
.btn5:active {
    transform: scale(0.96);
}
.btn5:disabled {
    pointer-events: none;
    opacity: 0.65;
}
</style>