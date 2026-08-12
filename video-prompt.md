# 视频关键帧方案生成提示词

这份文档用于生成与本项目相同视觉逻辑的预渲染视频：一台 Macintosh Classic 从画面中央的小尺寸开始，在单一连续镜头中旋转一整圈并同步放大，最后显示器屏幕严格正对镜头、填满画面，再由网页中的真实 HTML 页面交叉淡入。

## 先确定实现边界

视频生成模型不擅长稳定生成可读 UI、菜单文字和像素级一致的屏幕内容。因此推荐把效果拆成两部分：

1. 视频只生成 Macintosh 的旋转、放大和推进，最后停在正对屏幕的近景。
2. MacPaint 页面仍然使用 HTML/CSS/SVG 构建，在视频末尾通过透明度交叉淡入。

如果要求模型外形、接口和屏幕边框完全准确，单靠文本提示词不够。优先使用图生视频，并提供同一模型渲染出的首帧和尾帧参考图。更严格的商业项目应直接在 Blender、Cinema 4D 或 Unreal Engine 中渲染视频。

## 推荐输入材料

- 首帧：Macintosh 正面、小尺寸、严格位于画面中心，背景为均匀浅暖灰色。
- 尾帧：同一台 Macintosh 的屏幕正对镜头，屏幕区域放大到覆盖整个画面。
- 模型参考：`macintosh_classic_1991.glb` 的正面、侧面和背面截图。
- 屏幕内容参考：静止的黑白 MacPaint 风格人物插画。生成过程中不要改变屏幕图案。

## 中文主提示词

```text
一个 6 秒、16:9、单镜头、无剪辑的产品动画。画面背景始终是均匀的浅暖灰色，没有房间、地面线、装饰或文字。一台真实、准确、保存完好的 1991 Macintosh Classic 电脑位于画面正中央，米白色复古塑料外壳，真实比例和细节，屏幕内保持同一张静止的黑白 MacPaint 风格插画。

动画开始时，电脑以很小的尺寸出现在画面中心，正面朝向镜头。随后电脑围绕自身垂直轴连续、平滑地旋转完整 360 度，同时从第一帧开始持续向镜头靠近并放大。旋转和放大必须同时发生，不能先旋转再放大。运动采用平滑的缓入缓出，整个过程速度连续，没有停顿、跳变、切镜或镜头抖动。镜头本身固定，保持正视透视，画面中心不漂移。

在动画约 78% 的位置，电脑刚好完成整整一圈旋转，显示器正面与镜头完全平行，屏幕中心与画面中心精确重合。随后继续快速而平滑地向屏幕推进，电脑整体可以略微向画面下方移动，使显示器屏幕而不是整机几何中心对准画面中心。最后一帧中，显示器屏幕的内侧矩形完全正对镜头并扩大到覆盖整个 16:9 画面，为切换到网页 UI 留出干净、稳定、无畸变的末帧。

真实产品摄影质感，柔和均匀的工作室光线，细腻但克制的接触阴影，模型几何形状在全过程中保持一致。最后屏幕必须是正面，边缘水平和垂直，不允许透视歪斜。
```

## English master prompt

多数视频生成模型对英文运动描述的执行更稳定，可以优先使用这一版：

```text
A 6-second, 16:9, single continuous product shot with no cuts. The background remains a uniform light warm gray throughout, with no room, horizon line, decorations, captions, or extra objects. A physically accurate, well-preserved 1991 Macintosh Classic is placed exactly at the center of the frame, with an aged off-white plastic enclosure, correct proportions and details. Its display keeps the same static black-and-white MacPaint-style portrait for the entire shot.

At the beginning, the Macintosh is very small and front-facing in the center of the frame. It then rotates smoothly through exactly one full 360-degree turn around its own vertical axis while simultaneously moving toward the camera and growing larger from the very first frame. Rotation and scale-up happen together as one continuous coordinated motion, never as two separate phases. Use smooth ease-in and ease-out with continuous velocity, no pauses, jumps, cuts, camera shake, or framing drift. The camera stays fixed and front-facing.

At approximately 78 percent of the shot, the Macintosh completes exactly one full rotation and its display is perfectly front-facing and parallel to the camera. The center of the physical display aligns precisely with the center of the frame. It then continues pushing rapidly but smoothly into the display. The computer may shift slightly downward so the display, rather than the center of the whole computer body, stays centered. In the final frame, the inner display rectangle is perfectly frontal, with horizontal and vertical edges, and expands until it completely covers the 16:9 frame, creating a clean, stable, undistorted transition plate for a webpage UI.

Realistic product photography, soft even studio lighting, subtle contact shadow, consistent geometry and materials in every frame. The final display must face the camera exactly with no perspective skew.
```

## 负向提示词

```text
不要切镜，不要先旋转后放大，不要分段运动，不要相机环绕，不要相机抖动，不要焦距突变，不要背景变化，不要出现桌面或房间，不要额外物体，不要悬浮文字，不要水印，不要改变电脑型号，不要改变机身比例，不要增加键盘或鼠标，不要改变屏幕图案，不要屏幕闪烁，不要材质融化，不要几何变形，不要重复旋转，不要旋转不足一圈，不要在结尾停留于侧面或背面，不要让屏幕倾斜，不要让屏幕中心偏离画面中心。

no cuts, no separate rotation and zoom phases, no orbiting camera, no camera shake, no zoom lens jump, no changing background, no desk, no room, no extra objects, no floating text, no watermark, no model changes, no proportion changes, no keyboard, no mouse, no changing screen artwork, no screen flicker, no melting plastic, no geometry deformation, no extra rotation, no incomplete rotation, no side-facing final frame, no perspective-skewed display, no off-center display
```

## 时间轴约束

| 视频进度 | 画面要求 |
| --- | --- |
| 0% | 模型很小、正面朝向、严格居中 |
| 20% | 已开始旋转并同步缓慢放大，不改变画面中心 |
| 50% | 展示侧面或背面，继续接近镜头 |
| 62% | 开始更明显地加速放大，旋转仍在继续 |
| 78% | 恰好完成 360 度，屏幕完全正对镜头 |
| 82% | 屏幕中心精准对齐画面中心，准备交叉淡化 |
| 90%–100% | 屏幕矩形覆盖画面，保持稳定，供 HTML 页面接管 |

生成工具支持首尾关键帧时，把 0% 和 100% 的参考图锁定；支持运动笔刷或路径控制时，只给模型添加垂直轴旋转和朝镜头方向的推进，不要移动相机。

## 建议生成参数

- 时长：6–8 秒。
- 画幅：16:9。
- 分辨率：至少 1920×1080。
- 帧率：30 fps。
- 镜头：固定相机、单镜头、无剪辑。
- 运动强度：中等，优先保证模型一致性和末帧对齐。
- 参考图权重：中高；如果模型变形，继续提高参考图权重并降低运动强度。

## 为滚动拖动重新编码

普通视频的关键帧间隔很长，频繁修改 `video.currentTime` 时容易卡顿。生成完成后，可以把关键帧间隔缩短到 6 帧，并移除 B 帧：

```bash
ffmpeg -i generated.mov -an \
  -vf "fps=30,scale=1920:-2:flags=lanczos" \
  -c:v libx264 -pix_fmt yuv420p -profile:v high -preset slow -crf 20 \
  -g 6 -keyint_min 6 -sc_threshold 0 -bf 0 -movflags +faststart \
  macintosh-scroll.mp4
```

前端将滚动进度映射到视频时间：

```js
const progress = scrollY / (document.documentElement.scrollHeight - innerHeight);
video.currentTime = progress * video.duration;
```

实际实现时应在 `requestAnimationFrame` 中平滑目标时间，并在视频最后约 18% 的进度内同步淡入 MacPaint HTML 页面，以隐藏生成视频末帧与真实 UI 之间的细小差异。
