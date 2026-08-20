!!! abstract "You are here"
    **Module 04 — Actuators**  ·  **Unit 1 — The Cylinder**  ·  **Lesson 05 — The cylinder in motion**

# Lesson 05 — The cylinder in motion

> **Module 04 · Lesson 05** · *Mass and fluid stiffness, together, over a full cycle.*
> Every lesson so far took one snapshot — force, speed, friction, the stop. This one lets the cylinder *move*: the moving mass rides on the fluid spring you met in Module 03, and the two together set how the platform accelerates, settles, and rings. This is the cylinder as a living, dynamic thing — and it closes Module 04.
>
> **Learning outcome:** Combine the moving mass and the fluid's stiffness into the cylinder's equation of motion, find its natural frequency, and explain what governs how it accelerates and settles over a pick cycle — and why keeping air out protects that response.

---

## 1. Why This Matters

Up to now the cylinder has been frozen at an instant: this much force, that much speed. But the platform's real job is *motion* — accelerate the load, cruise, decelerate, and settle to a stop within ±1 mm. Doing that well is not just about having enough force; it is about how the cylinder *responds*. Push a load on a stiff support and it moves crisply; push it on a springy one and it lags, overshoots, and bounces before it settles. The cylinder's support is the trapped oil — a spring, as Module 03 showed — and the load is a mass on that spring.

So the decision this lesson settles is the dynamic one: **how does the cylinder behave when it moves — how fast can it accelerate and how quickly does it settle — and what sets those limits?** The answer ties the whole module together: the force from Lesson 01, the speed from Lesson 02, the friction from Lesson 03, and the fluid stiffness from Module 03 all combine into one equation of motion. And it reveals why the air you kept out of the oil matters not just for holding, but for moving well.

## 2. Physical Intuition

A mass on a spring has a natural rhythm — pull it and let go and it oscillates at a frequency set by how stiff the spring is and how heavy the mass is: stiffer and lighter means faster, softer and heavier means slower. The cylinder is exactly this system. The **mass** is the piston, rod, and whatever it carries; the **spring** is the trapped oil, whose stiffness you can now compute from its bulk modulus. Give the piston a shove — a step of pressure — and it does not jump instantly to the new position; it accelerates, and because the oil-spring stores and returns energy, it can overshoot and ring before settling.

Two things follow. First, there is a **natural frequency**: try to command motion faster than it and the cylinder cannot keep up — it lags and oscillates. This is the bandwidth limit of the whole platform. Second, the **stiffness matters twice over**: the same bulk modulus that lets the platform *hold* to ±1 mm also sets how quickly it *settles* to that ±1 mm after a move. Let air soften the oil and the natural frequency drops, the response slows, and the overshoot grows — so the air you excluded in Module 03 pays off again here, in motion.

## 3. The Idea You Now Need

Newton's law for the piston gathers every force from this module into one equation of motion:

$$ m\,\ddot{x} = p\,A_b - F_f(\dot{x}) - F_\text{load} $$

The driving force $p\,A_b$ (Lesson 01) fights friction $F_f$ (Lesson 03) and the load, and whatever is left accelerates the mass $m$. The pressure $p$ is not free to jump, because the oil is compressible: squeezing the trapped volume $V$ raises pressure by $B_e$ per fractional squeeze, which makes the oil act as a spring of stiffness

$$ k = \frac{B_e\,A_b^2}{V} $$

Mass on that spring gives a **natural frequency**:

$$ \omega_n = \sqrt{\frac{k}{m}}, \qquad f_n = \frac{\omega_n}{2\pi} $$

For the platform at mid-stroke, $k \approx 23.1\ \text{MN/m}$. Carrying the two-tonne load that is $f_n \approx 17\ \text{Hz}$; for the bare 3 kg tool it is far higher (~440 Hz), because a lighter mass on the same spring is quicker. That $f_n$ is the platform's speed limit for clean motion — and it falls if the oil softens.

## 4. Visual Explanation

<figure markdown>
  ![On the left, the cylinder redrawn as a mass on a spring: the moving mass m is the piston, rod and load; the spring is the trapped oil with stiffness k equals B_e times A squared over V, about 23.1 meganewtons per metre. On the right, a step response over a pick cycle: commanded a step, the piston accelerates, slightly overshoots, and rings before settling to the target within plus or minus one millimetre. Two curves compare a stiff clean-oil response, which settles quickly at about 17 hertz, with a softer aerated-oil response, which is slower and overshoots more at about 10 hertz — showing that keeping air out gives faster, cleaner motion.](assets/m04-l5-dynamics.svg){ width="760" }
</figure>

On the left, the cylinder stripped to its essence: a **mass on a fluid spring**. The spring stiffness is the bulk modulus from Module 03 turned into $k = B_e A_b^2/V \approx 23.1\ \text{MN/m}$. On the right, what that system does when you command a move: it accelerates, may **overshoot**, and **rings** at its natural frequency before settling into the ±1 mm band. The two curves are the point — stiff clean oil (~17 Hz) settles quickly with little overshoot; softer aerated oil (~10 Hz) is slower and rings more. Keeping air out does not just hold the load; it makes every move faster and cleaner.

## 5. Engineering Example

A crane operator knows this in their hands. Lift a heavy load on a long, springy cable and it sways and bobs; every move has to be gentle and slow, waiting for the swing to die before the next. Shorten and stiffen the support and the same load handles crisply. The cable is the fluid spring; the load is the mass; the sway is the natural frequency. Robots and machine tools obsess over "structural stiffness" for the same reason — a stiffer machine has a higher natural frequency, so it can move faster and settle sooner without chatter. Your platform's stiffness lives in its oil, which is why its condition governs not just precision at rest but speed in motion.

## 6. Worked Example

<div class="worked" markdown="1">

**Given** — the platform's cylinder as a mass–spring system at mid-stroke:

- Fluid spring stiffness $k = B_e A_b^2/V \approx 23.1\ \text{MN/m}$ (with $B_e = 1.5\ \text{GPa}$, $A_b = 1963.5\ \text{mm}^2$, $V \approx 0.25\ \text{L}$)
- Moving mass: the two-tonne load, $m \approx 2000\ \text{kg}$

**Find** — the natural frequency carrying the load, how it changes for the bare tool, and what entrained air does to it.

**Assumptions**

- Small motions about mid-stroke; stiffness roughly constant there.
- Light damping, so $f_n \approx \tfrac{1}{2\pi}\sqrt{k/m}$.

**Solution**

$$ f_n = \frac{1}{2\pi}\sqrt{\frac{k}{m}} = \frac{1}{2\pi}\sqrt{\frac{23.1\times10^6}{2000}} \approx 17\ \text{Hz} $$

For the bare 3 kg tool the same spring gives $f_n = \tfrac{1}{2\pi}\sqrt{23.1\times10^6/3} \approx 440\ \text{Hz}$ — far quicker, because the mass is tiny. Now soften the oil with entrained air so $B_e$ falls to 0.5 GPa: $k$ falls by the same factor to ~7.8 MN/m, and the loaded natural frequency drops to

$$ f_n = \frac{1}{2\pi}\sqrt{\frac{7.8\times10^6}{2000}} \approx 10\ \text{Hz} $$

**Result**

$$ \boxed{f_n \approx 17\ \text{Hz loaded (clean oil)};\quad \approx 440\ \text{Hz for the bare tool};\quad \approx 10\ \text{Hz if air softens the oil}} $$

**Engineering Interpretation** — The heavy load, on the oil spring, gives a natural frequency around 17 Hz: that is the platform's dynamic speed limit — command motion much faster and it lags and rings. The bare tool is far quicker, so *what you carry* sets the dynamics as much as the cylinder does. And the last number is the lesson of the whole fluid module coming home: let air drop the effective stiffness and the natural frequency falls to ~10 Hz, so the platform moves more slowly and settles less cleanly. Force, speed, friction, stopping, and now dynamics — the cylinder is fully characterised, and every one of them leans on the fluid you specified in Module 03.

</div>

## 7. Interactive Demonstration

<iframe src="demos/lesson05_dynamics.html" title="The cylinder in motion — mass on a fluid spring" style="width:100%;height:860px;border:1px solid #e2e8f0;border-radius:12px"></iframe>

Set the moving mass and the oil's effective stiffness (add air to soften it) and watch two things move together: the natural frequency, and the step response as the piston is commanded to a new position. Heavier loads and softer oil both lower the frequency, slow the response, and grow the overshoot — see how clean, quick motion needs a stiff, air-free oil and rewards a lighter tool.

## 8. Coding Exercise

```python
import math
Ab = math.pi/4 * 0.050**2      # bore area, m^2
V  = 0.25e-3                    # chamber volume at mid-stroke, m^3 (~0.25 L)

def natural_freq(Be, m):
    k = Be * Ab**2 / V          # fluid spring stiffness, N/m
    return (1/(2*math.pi)) * math.sqrt(k / m)

print(round(natural_freq(1.5e9, 2000), 1), "Hz  loaded, clean-ish oil")   # ~17
print(round(natural_freq(1.5e9, 3), 1),    "Hz  bare 3 kg tool")          # ~440
print(round(natural_freq(0.5e9, 2000), 1), "Hz  loaded, aerated oil")     # ~10
```

**Your task:** confirm ~17 Hz loaded and ~10 Hz with aerated oil. Then: if you wanted the loaded natural frequency to be at least 20 Hz, would you work on the oil, the mass, or the cylinder geometry — and what does each cost you elsewhere?

## 9. Knowledge Check

Formative — unlimited attempts, immediate feedback; does not affect your grade.

<iframe src="quizzes/lesson05_quiz.html" title="The cylinder in motion — knowledge check" style="width:100%;height:900px;border:1px solid #e2e8f0;border-radius:12px"></iframe>

1. In the cylinder's equation of motion, what does the left side ($m\ddot{x}$) represent, and what three forces act on the right?
2. Where does the "spring" in the mass–spring picture come from?
3. What is the natural frequency, and why does it matter for how fast the platform can move?
4. Entrained air lowers the effective bulk modulus. What does that do to the natural frequency and the settling?
5. The bare tool has a much higher natural frequency than the loaded platform. Why?

## 10. Challenge Problem

The platform must make a 200 mm move and settle to ±1 mm as quickly as possible, but with the two-tonne load it overshoots and takes too long to stop ringing. Explain, using the natural frequency and damping, why the heavy load makes this hard, and give three levers an engineer could pull — across the oil, the mechanism, and the way the cylinder is driven — to settle faster. Which of these does Module 03's cleanliness and air control already help?

## 11. Common Mistakes

- **Treating the cylinder as rigid.** The oil is a spring; the piston is a mass on it. Ignore that and you cannot explain overshoot, ringing, or the bandwidth limit.
- **Forgetting the load changes the dynamics.** The same cylinder is quick with a light tool and sluggish with a heavy load — the mass sets the natural frequency as much as the stiffness does.
- **Separating "holding" from "moving."** The bulk modulus governs both: it sets the ±1 mm hold *and* the speed and cleanliness of settling. Air hurts both.
- **Chasing more force for a settling problem.** Overshoot and ringing are about stiffness, mass, and damping — not force. More push does not settle a springy system faster.

## 12. Key Takeaways

**The decision you can now make:** treat the cylinder as a mass on a fluid spring, find its natural frequency, and recognise what sets how quickly and cleanly it moves and settles.

- The cylinder's motion obeys $m\ddot{x} = p\,A_b - F_f(\dot x) - F_\text{load}$ — the whole module's forces in one equation.
- The trapped oil is a spring, $k = B_e A_b^2/V \approx 23.1\ \text{MN/m}$; mass on it gives a **natural frequency** $f_n = \tfrac{1}{2\pi}\sqrt{k/m}$.
- Loaded (2 tonnes) that is **~17 Hz** — the platform's speed limit for clean motion; the bare 3 kg tool is far quicker (~440 Hz).
- **Air softens the oil and lowers $f_n$** (~17 → ~10 Hz), slowing the response and growing overshoot — so keeping air out helps motion, not just holding.
- **Module 04 is complete.** The cylinder is fully characterised — force, speed, usable force, a safe stop, and its dynamics. **Module 05 now sizes the power unit** that must feed all of it.

## AI Learning Companion

Copy a prompt into an AI assistant.

**Deepen** — the mass–spring cylinder

```
Explain how a hydraulic cylinder behaves as a mass on a fluid spring: the moving mass, the trapped-oil stiffness k = Be*A^2/V, and the natural frequency wn = sqrt(k/m). Using Be = 1.5 GPa, A = 1963.5 mm^2, V = 0.25 L, compute the stiffness, then the natural frequency for a 2000 kg load and for a 3 kg tool, and explain why they differ so much.
```

**Challenge** — stiffness governs motion

```
Explain why the effective bulk modulus of hydraulic oil affects not just how precisely a cylinder holds a load, but how quickly and cleanly it settles after a move. How does entrained air, by lowering the effective bulk modulus, reduce the natural frequency and increase overshoot and settling time? Tie this back to why hydraulic systems bleed air.
```

**Explore** — the full equation of motion

```
Write out the coupled dynamics of a hydraulic cylinder: the mechanical equation m*x'' = p*A - friction - load, and the pressure/flow equation from compressibility, p' = (Be/V)*(Q_in - A*x'). Explain how these combine into a second-order system, what sets its natural frequency and damping, and how a control engineer would use this model to design position control.
```

## Global Learning Support

Need this lesson in another language? Copy the prompt into an AI assistant. English remains the authoritative source.

**Supported languages (initial):** English · Español · 中文 (Simplified) · Türkçe

```
I just completed Module 04 Lesson 05 — The cylinder in motion.
Explain this lesson in [Spanish / Simplified Chinese / Turkish], keeping common engineering terms in English where usual.
Then give me: a short summary, three practice questions, and one challenge problem.
```

---

*Module 04 is complete — the cylinder is fully characterised, from static force to full dynamic response, every part of it resting on the fluid specified in Module 03. Next: Module 05 — Power Units, where you size the pump, motor, relief, and reservoir that must supply everything this cylinder demands.*
