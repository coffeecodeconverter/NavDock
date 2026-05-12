
# Building a Fluid macOS-Style Dock with Vanilla JavaScript and CSS

There is something undeniably satisfying about the macOS Dock. The way icons swell as your cursor approaches, the subtle "breathing" room between elements, and the glass-morphism aesthetic create a premium user experience. 

In this article, we’ll break down how to recreate this fluid, interactive navigation system using only standard web technologies.

Try the Live Project Here: <br>
**https://turnerworks.uk/portfolio/macdocknavsystem/index.html**


<img height="600" alt="image" src="https://github.com/user-attachments/assets/85ec4186-2e5c-421e-abc1-b9770cd20867" />


---

## 1. The Design Philosophy: Glassmorphism & Motion
The aesthetic relies on **Backdrop Filters**. By using `backdrop-filter: blur(25px)`, the dock doesn't just sit on top of the background; it interacts with it, appearing like frosted glass.

### Key CSS Features:
* **Variable-Driven Layout:** Using CSS variables (`--base-size`) allows for real-time scaling without manual recalculations.
* **Dynamic Origins:** Depending on whether the dock is at the top, bottom, or sides, the `transform-origin` changes to ensure icons expand *away* from the edge and toward the center of the screen.

---

## 2. The Physics of the "Swell" (The Math)
The most challenging part of a macOS dock is the magnification. We don't want a binary "hover or not" state; we want a bell curve of expansion.

<img  height="400" alt="image" src="https://github.com/user-attachments/assets/3774ee0b-2dcb-475e-9206-3bb96f3debb9" />


### The Cosine Curve
To achieve a smooth magnification effect, the code calculates the distance ($d$) between the mouse and the center of each icon. If the distance is within the `hoverRange`, we apply a cosine interpolation:

$$target = \cos\left(\frac{d}{hoverRange} \cdot \frac{\pi}{2}\right)$$

This ensures that the icon directly under the cursor is at $100\%$ magnification, while its neighbors scale down gracefully to $0\%$ extra magnification as the distance increases.

---

## 3. Smooth Interpolation (The Feel)
If we mapped the mouse position directly to the icon size, the movement would feel jittery. To solve this, we use **Linear Interpolation (Lerp)**.


find more: <br>
https://turnerworks.uk/index.html <br>
https://github.com/turnerworks <br>
<Br>
Instead of setting the scale instantly, we update it by a fraction of the distance between the *current* scale and the *target* scale:

```javascript
currentScales[index] += (target - currentScales[index]) * animationSmoothness;
