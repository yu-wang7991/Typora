#### 帖子图片布局

```html
<view class="img-list">
      <view
        :class="[
          'img-item',
          'item-col-' + (content.images.length > 3 ? 3 : content.images.length),
        ]"
        v-for="(itm, i) in content.images"
        :key="itm"
        @tap.stop="previewPicList(content.images, i)"
      >
        <view class="inner">
          <image :src="itm.url" mode="aspectFill"></image>
        </view>
      </view>
    </view>
 .img-list {
    width: 100%;
    display: flex;
    flex-wrap: wrap;
    overflow: hidden;

    .img-item {
      position: relative;
      height: 0;

      .inner {
        width: 100%;
        position: absolute;
        height: 100%;
        padding: 5rpx;
        top: 0;
        left: 0;
        box-sizing: border-box;
      }

      image {
        display: block;
        width: 100%;
        height: 100%;
        background-position: center center;
      }
    }

    .item-col-1 {
      width: 100%;
      padding-top: 100%;
    }

    .item-col-2 {
      width: 50%;
      padding-top: 50%;
    }

    .item-col-3 {
      width: 33.3333%;
      padding-top: 33.3333%;
    }
  }
  previewPicList(list, idx) {
      const currList = list.map((a) => a.url);
      uni.previewImage({
        urls: currList,
        current: currList[idx],
      });
    },
```

