# FLIP动画实现演示
FLIP动画是一种高性能的Web动画技术，它通过记录元素的初始和最终位置，然后应用反向变换来创建流畅的动画效果。这种技术特别适合处理布局变化时的元素动画。

```shell
npm i -g http-server

http-server
```

## FLIP动画原理
**F - First**
获取元素的初始位置（位置、尺寸等）。

**L - Last**
将元素移动到最终位置，并获取最终的位置信息。

**I - Invert**
计算初始位置和最终位置的差异，应用反向变换（如translate），使元素看起来仍在原始位置。


**P - Play**
播放动画：移除反向变换，使元素过渡到最终位置。

性能优势： FLIP动画使用transform和opacity属性实现动画，这些属性可以由GPU加速处理，避免重排和重绘，从而实现60fps的流畅动画。

```js
// FLIP动画核心代码示例
function flipAnimation(element, newPosition) {
// First: 获取初始位置
const first = element.getBoundingClientRect();

    // Last: 应用最终状态并获取位置
    applyFinalPosition(element, newPosition);
    const last = element.getBoundingClientRect();

    // Invert: 计算差异并应用反向变换
    const invertX = first.left - last.left;
    const invertY = first.top - last.top;

    element.style.transform = `translate(${invertX}px, ${invertY}px)`;

    // Play: 执行动画
    requestAnimationFrame(() => {
        element.style.transition = 'transform 0.5s ease';
        element.style.transform = '';

        // 动画完成后清理
        element.addEventListener('transitionend', () => {
            element.style.transition = '';
        }, { once: true });
    });
}
```
