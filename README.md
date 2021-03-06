# InfiniteCards
[![License](https://img.shields.io/badge/license-Apache%202-4EB1BA.svg)](https://www.apache.org/licenses/LICENSE-2.0.html)

An infinite card switching UI for Android, support custom animation

## Screenshot
![](./screenshot/sample.gif)

## Gradle via JCenter
``` groovy
compile 'com.bakerj:infinite-cards:1.0.0'
```

## Attrs
- animType : animation type
  - front : move the selected card to first
  - switchPosition : move the selected card to first, and the first card to the selected position
  - frontToLast : move the first card to last position
- cardRatio : ratio of the card
- animDuration : duration of each card's animation
- animAddRemoveDelay : delay of animation of add and remove between each card
- animAddRemoveDuration : duration of add and remove each card's animation

## How to use
### layout in xml
```xml
<com.bakerj.infinitecards.InfiniteCardView
        android:id="@+id/view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        infiniteCard:animDuration="1000"
        infiniteCard:cardRatio="1"/>
```

### Animation transformers and interpolators
#### Default
If you just use all default animations, just do nothing.
```java
mCardView.setAnimInterpolator(new LinearInterpolator());
mCardView.setTransformerToFront(new DefaultTransformerToFront());
mCardView.setTransformerToBack(new DefaultTransformerToBack());
mCardView.setZIndexTransformerToBack(new DefaultZIndexTransformerCommon());
```
#### Customisation
```java
mCardView.setTransformerToBack(new AnimationTransformer() {
    @Override
    public void transformAnimation(View view, float fraction, int cardWidth, int cardHeight, int fromPosition, int toPosition) {
        int positionCount = fromPosition - toPosition;
        float scale = (0.8f - 0.1f * fromPosition) + (0.1f * fraction * positionCount);
        ViewHelper.setScaleX(view, scale);
        ViewHelper.setScaleY(view, scale);
        if (fraction < 0.5) {
            ViewCompat.setRotationX(view, 180 * fraction);
        } else {
            ViewCompat.setRotationX(view, 180 * (1 - fraction));
        }
    }

    @Override
    public void transformInterpolatedAnimation(View view, float fraction, int cardWidth, int cardHeight, int fromPosition, int toPosition) {
        int positionCount = fromPosition - toPosition;
        float scale = (0.8f - 0.1f * fromPosition) + (0.1f * fraction * positionCount);
        ViewHelper.setTranslationY(view, -cardHeight * (0.8f - scale) * 0.5f - cardWidth * (0.02f *
                fromPosition - 0.02f * fraction * positionCount));
    }
});
mCardView.setZIndexTransformerToBack(new ZIndexTransformer() {
    @Override
    public void transformAnimation(CardItem card, float fraction, int cardWidth, int cardHeight, int fromPosition, int toPosition) {
        if (fraction < 0.4f) {
            card.zIndex = 1f + 0.01f * fromPosition;
        } else {
            card.zIndex = 1f + 0.01f * toPosition;
        }
    }

    @Override
    public void transformInterpolatedAnimation(CardItem card, float fraction, int cardWidth, int cardHeight, int fromPosition, int toPosition) {

    }
});
```
### *License*

InfiniteCards is released under the [Apache 2.0 license](license.txt).

```
Copyright 2017 BakerJ.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at following link.

     http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
