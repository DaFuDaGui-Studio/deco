# decollte


### function slideIndicatorCarousel (可自動 / 滑動 / 手動 輪播)
* `x-data="slideIndicatorCarousel"` 
* 自動播放 `x-data="slideIndicatorCarousel(true)"` 就帶 true (預設不自動撥放)
* 搭配 daisyUI
* 輪播的div  class="carousel" x-ref="carousel"
* 輪播的每個項目 class="carousel-item"
* 下面有點點的 每個點點 @click="goTo('第幾個item')"


## gridSlideCarousel 使用說明

### 功能介紹
支援自動 / 滑動 / 手動輪播功能，並可設定圖片欄數、列數、顯示點點、箭頭、滑動切換、暫停播放等參數。

### 使用方式
使用 `x-data="gridSlideCarousel(options)"` 初始化，`options` 為設定參數物件。

### 可用參數（預設為 false 或預設值）
- `dots`：是否顯示底部點點導航，預設 `false`
- `arrows`：是否顯示左右箭頭，預設 `false`
- `touchSwipe`：是否啟用觸控滑動切換，預設 `false`
- `pauseOnHover`：滑鼠懸停是否暫停自動播放，預設 `false`
- `autoPlay`：是否啟用自動播放，預設 `false`
- `columns`：圖片欄數，可設定數字或物件，預設 `{ mobile: 1, tablet: 2, desktop: 4 }`  
  - 物件可只設定部分欄位，未設定欄位會使用預設值補齊
- `rows`：圖片列數，預設 `2`
- `images`：圖片陣列，格式如下：
  ```js
  [
    { id: '1', img: 'img1.jpg', alt: '圖片說明1' },
    { id: '2', img: 'img2.jpg', alt: '圖片說明2' },
    // ...
  ]

### 使用範例
```html
<div
  x-data="gridSlideCarousel({ 
    dots: true, 
    arrows: true, 
    columns: { mobile: 1, tablet: 2, desktop: 4 }, 
    rows: 1,
    images: [
      { id: '1', img: 'img1.jpg', alt: '圖1' },
      { id: '2', img: 'img2.jpg', alt: '圖2' },
      { id: '3', img: 'img3.jpg', alt: '圖3' },
      { id: '4', img: 'img4.jpg', alt: '圖4' }
    ],
  })"
  x-init="init($el)"
  class="relative"
  <!-- 使用 pauseOnHover 功能時，才需要加上這兩個事件 -->
  @mouseenter="pauseOnHover ? stopAutoPlay() : null"
  @mouseleave="pauseOnHover ? startAutoPlay() : null"
>
  <!-- 輪播容器 -->
  <div class="overflow-hidden">
    <div
      class="flex transition-transform duration-500"
      :style="`width: ${pagedImages.length * 100}%; transform: translateX(-${currentPage * (100 / pagedImages.length)}%)`"
    >
      <template x-for="(page, pageIndex) in pagedImages" :key="pageIndex">
        <!-- 用 grid 做頁面，調整列數跟欄數 -->
        <div
          class="grid gap-2 p-2"
          :style="`
            width: ${100 / pagedImages.length}%;
            grid-template-columns: repeat(${Number(columns)}, 1fr);
            grid-template-rows: repeat(${Number(rows)}, 1fr);
          `"
        >
          <template x-for="item in page" :key="item.id">
            <div class="p-2 bg-gray-100 rounded">
              <img :src="item.img" :alt="item.alt" class="w-auto h-full" />
              <p class="text-sm text-center mt-1" x-text="item.alt"></p>
            </div>
          </template>
        </div>
      </template>
    </div>
  </div>

  <!-- 左右箭頭 -->
  <!-- 使用 arrows 再加，也可以包一層後一起位移 -->
  <button
    @click="goToPage(currentPage - 1)"
    class="absolute top-1/2 left-1 z-10 bg-white p-1 rounded shadow md:hidden"
  >
    ←
  </button>
  <button
    @click="goToPage(currentPage + 1)"
    class="absolute top-1/2 right-1 z-10 bg-white p-1 rounded shadow md:hidden"
  >
    →
  </button>

  <!-- 點點 -->
  <!-- 使用 dots 再加，也可以包一層後一起位移 -->
  <div class="flex justify-center mt-2 space-x-2 md:hidden">
    <template x-for="(dot, index) in pagedImages.length" :key="index">
      <button
        @click="goToPage(index)"
        :class="index === currentPage ? 'bg-black' : 'bg-gray-400'"
        class="w-[0.5rem] h-[0.5rem] sm:w-[0.4rem] sm:h-[0.4rem] rounded-full"
      ></button>
    </template>
  </div>
</div>
```

## 共用 class
- `custom-heading-en`：標題英文，預設置中，電腦版可選加上`md:text-left`
- `custom-heading-zh`：標題中文，預設置中，電腦版可選加上`md:text-left`
