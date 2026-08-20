!!! abstract "You are here"
    **Module 10 — Control Systems**  ·  **Unit 2 — Shaping the Controller**  ·  **Lesson 02 — Proportional control**

# Lesson 02 — Proportional control

> **Module 10 · Lesson 02** · *Command in proportion to the error.*
> The simplest controller sets the command proportional to the error. This lesson tunes that single gain — trading response speed against steady-state droop, and bounded above by the oil-spring resonance.
>
> **Learning outcome:** Design a proportional controller for the platform — choose the gain to balance response speed and steady-state droop while staying below the oil-spring resonance that limits how high the gain can go.

---

## 1. Why This Matters

Feedback gave the platform the *structure* to correct itself; now it needs a *rule* — a specific function turning error into command. The simplest and most fundamental is proportional control: command in proportion to how far off you are. Big error, big push; small error, gentle push; on target, no push. It is the natural first move, it is at the heart of nearly every controller ever built, and understanding its single knob — the gain — teaches the central tension of all control: you want to respond fast and hold tight, but push the gain too hard and the system shakes itself apart on its own resonance. Master the proportional gain and you understand the trade-off every controller lives inside.

So the decision this lesson makes is: **how do we set the proportional gain — high enough to be fast and tight, low enough to stay stable?** Turn the gain up and the platform responds faster (its settling time shrinks) and holds tighter under disturbance (its droop shrinks); both improve together, which tempts you to keep turning. But the platform has an oil spring that rings at about 12 Hz, and as the gain rises the control loop gets faster until it starts exciting that resonance — first a little overshoot, then heavy ringing, then a platform oscillating out of control. So the gain is bounded above by the resonance and bounded below by the droop you can tolerate, and the design is finding the window between. For this platform that window is comfortable — a gain around 0.1 to 0.15 gives a response time under a tenth of a second and a droop well under a millimetre, with the resonance still safely out of reach.

## 2. Physical Intuition

Picture the proportional controller as a spring pulling the platform toward the target. The further from target, the harder the pull; right at target, no pull. A stiffer spring — higher gain — snaps the platform to target faster and resists being pushed off it more firmly. That is the appeal: crank the gain and everything seems to get better, quicker arrival and tighter holding at once. And for a while it does. At low gain the platform crawls to target, sluggish; at moderate gain it moves briskly and settles cleanly; the response time shrinks in direct proportion to the gain.

But the platform is not a simple mass on your control spring — it has its *own* spring inside, the compressible oil, which rings at about 12 Hz. At low and moderate control gain the loop is slow compared to that ring, so the oil spring stays quiet and the motion is smooth. As you keep raising the gain, the control loop gets faster and faster until it begins to slap the oil spring at its resonant rhythm — and then, like pushing a swing at just the right moment, the oscillation builds. A little more gain gives overshoot; more gives sustained ringing; too much and the platform shakes violently, the control fighting a resonance it is itself feeding. This is the universal ceiling on proportional gain: not arithmetic, but the physical resonance of the thing being controlled. And it explains why proportional control, for all its simplicity, cannot both hold perfectly and stay calm — there is always a little residual droop it leaves behind, because erasing it entirely would demand a gain the resonance forbids.

## 3. The Idea You Now Need

Proportional control sets the command in proportion to the error:

$$ u = K_p\,e = K_p\,(x_\text{target}-x) $$

Raising $K_p$ improves two things at once. **Response speed** — the closed-loop settling follows a time constant that shrinks with gain:

$$ \tau = \frac{1}{v_\text{max}\,K_p} \;:\quad K_p=0.05\to0.24\ \text{s}, \quad K_p=0.15\to0.08\ \text{s} $$

**Steady-state droop** — the residual error under a sustained disturbance $d$ also shrinks with gain:

$$ e_\text{ss} = \frac{d}{v_\text{max}\,K_p} \;:\quad K_p=0.05\to0.71\ \text{mm}, \quad K_p=0.15\to0.24\ \text{mm} $$

To hold ±1 mm against a 3 mm/s disturbance needs only $K_p>d/v_\text{max}=0.035$, so $K_p\ge0.05$ already has margin. But raising $K_p$ cannot go on forever — the loop bandwidth $\approx v_\text{max}K_p$ must stay well below the oil-spring resonance $\omega_n\approx77$ rad/s (12 Hz), or the loop excites it:

$$ K_p\ \text{small} \to \text{sluggish}; \quad K_p\approx0.1\text{–}0.15 \to \text{fast, well-damped}; \quad K_p\gtrsim0.3 \to \text{rings, then oscillates} $$

So the proportional design is a window: above the droop limit, below the resonance limit. Its one lasting flaw — the residual droop — is what integral action removes in Lesson 03.

## 4. Visual Explanation

<figure markdown>
  ![Proportional control shown three ways. On the left, the control law: the command equals the gain times the error, drawn as a spring pulling the platform toward the target, stiffer at higher gain. In the centre, step responses at three gains: a low gain rises slowly and smoothly to target; a medium gain around 0.15 rises quickly with a small overshoot and settles; a high gain around 0.4 overshoots massively and rings with growing oscillation as it excites the 12 Hz oil spring. On the right, two small plots versus gain: settling time falling as gain rises, and steady-state droop falling as gain rises, with a shaded forbidden zone at high gain marked resonance limit where the loop goes unstable. A note reads: usable window Kp about 0.1 to 0.15.](assets/m10-l2-proportional.svg){ width="720" }
</figure>

Read the three views as the whole trade-off. The **control law** on the left is the spring metaphor: command proportional to error, stiffer with gain. The **step responses** in the centre show what gain does: too low and it crawls; around 0.15 it snaps to target with a slight, well-damped overshoot; too high and it excites the oil spring into ringing that grows rather than settles. The **trade-off plots** on the right show why you cannot simply maximise gain: settling time and droop both fall as gain rises — good — but a forbidden zone opens at high gain where the resonance takes over. The usable window sits between the droop you will accept and the resonance you must avoid, and for this platform that window comfortably contains a fast, tight, stable response. The one thing no gain fixes is the small droop that remains — the motivation for the next lesson.

## 5. Engineering Example

Proportional control is the workhorse first stage of almost every real controller — motor drives, temperature loops, flight surfaces, hydraulic servos — because it is simple, intuitive, and captures most of the benefit of feedback. Practising engineers reach for it first, raise the gain until the response is crisp, and back off when they see overshoot creeping in, using exactly the resonance ceiling this lesson describes as the stop sign. The residual droop under load is a well-known limitation: a proportionally-controlled machine holding a position under sustained force always sits slightly off target, by an amount inversely proportional to the gain, which is why proportional-only control is fine for many jobs but not for ones like the platform's that demand exactness under load. Recognising the resonance as the gain ceiling is also what separates a controls engineer from someone who just turns the knob up until it screams — the ceiling is a physical property of the plant, knowable in advance from the oil-spring frequency, so the safe gain can be estimated before the machine ever runs. The proportional gain is where control design starts; knowing its two limits is where control judgement starts.

## 6. Worked Example

<div class="worked" markdown="1">

**Given** — platform with $v_\text{max}=85$ mm/s and a ~12 Hz ($\omega_n=77$ rad/s) oil-spring resonance; a 3 mm/s sustained disturbance; ±1 mm target.

**Find** — the minimum gain to hold ±1 mm, the resulting speed and droop at $K_p=0.15$, and why the gain cannot simply be maximised.

**Assumptions**

- Closed-loop time constant $\tau=1/(v_\text{max}K_p)$; droop $e_\text{ss}=d/(v_\text{max}K_p)$; bandwidth $\approx v_\text{max}K_p$.

**Solution**

$$ K_p > \frac{d}{v_\text{max}} = \frac{3}{85} = 0.035 \;\Rightarrow\; \text{choose } K_p=0.15 $$
$$ \tau = \frac{1}{85\times0.15} = 0.078\ \text{s}, \qquad e_\text{ss} = \frac{3}{85\times0.15} = 0.24\ \text{mm} $$
$$ \text{bandwidth}\approx 85\times0.15 = 12.75\ \text{rad/s} \ll 77\ \text{rad/s (resonance)} \;\checkmark $$

**Result**

$$ \boxed{K_p=0.15:\ \tau\approx0.08\text{ s},\ \text{droop }0.24\text{ mm},\ \text{well below resonance}} $$

**Engineering Interpretation** — At $K_p=0.15$ the platform settles in under a tenth of a second and holds within a quarter-millimetre of target under the disturbance — fast and tight, comfortably inside ±1 mm. And the loop bandwidth, about 13 rad/s, sits far below the 77 rad/s resonance, so the oil spring stays quiet: no ringing, good damping. This is the sweet spot. Now see why you cannot just keep raising the gain: at $K_p=0.4$ the bandwidth climbs toward the resonance and the response overshoots wildly and oscillates — the loop is now exciting the oil spring it should be leaving alone. So the gain is genuinely bounded on both sides: too low leaves excessive droop, too high courts resonant instability, and the art is picking the middle. Notice the residual droop of 0.24 mm — small, within spec, but not zero. Proportional control cannot make it zero, because a nonzero holding command against the disturbance requires a nonzero error to generate it. For a job that must be *exactly* on target under load, that residual is unacceptable, and erasing it needs a new ingredient — the integral term of Lesson 03 — which accumulates the small error away without demanding more gain.

</div>

## 7. Interactive Demonstration

<iframe src="demos/lesson02_proportional.html" title="Proportional control — tune the gain" style="width:100%;height:900px;border:1px solid #e2e8f0;border-radius:12px"></iframe>

Turn the proportional gain up and down and watch the step response — sluggish at low gain, crisp in the middle, ringing into oscillation as the gain approaches the resonance. See the settling time and droop fall as you raise the gain, until the oil spring takes over.

## 8. Coding Exercise

```python
vmax, wn, zeta, dt = 85.0, 77.0, 0.12, 5e-5

def step(Kp, target=5.0, T=4.0):
    x=v=w=t=0.0; peak=0.0
    while t < T:
        u = Kp*(target - x)                       # proportional law (small-signal)
        dv, dw = w, wn*wn*(vmax*u - v) - 2*zeta*wn*w
        v += dv*dt; w += dw*dt; x += v*dt; t += dt
        peak = max(peak, x)
        if abs(x) > 1e3: return None              # loop diverged - > unstable
    return (peak-target)/target*100               # % overshoot

for Kp in (0.05, 0.15, 0.4):
    os = step(Kp)
    verdict = "unstable (diverges)" if os is None else f"overshoot {max(os,0):.0f}%"
    print(f"Kp={Kp}: {verdict},  tau={1/(vmax*Kp):.3f}s,  droop(3mm/s)={3/(vmax*Kp):.2f}mm")
```

**Your task:** confirm 0.15 is crisp while 0.4 rings hard. Then answer in a comment: both the settling time and the droop improve as you raise the gain — so what single thing stops you from raising it without limit?

## 9. Knowledge Check

Formative — unlimited attempts, immediate feedback; does not affect your grade.

<iframe src="quizzes/lesson02_quiz.html" title="Proportional control — knowledge check" style="width:100%;height:900px;border:1px solid #e2e8f0;border-radius:12px"></iframe>

1. What is the proportional control law, and what does the gain do?
2. How do response speed and droop change as the gain rises?
3. What sets the upper limit on the proportional gain?
4. What minimum gain holds ±1 mm against a 3 mm/s disturbance, and what does $K_p=0.15$ give?
5. Why does proportional control always leave a small residual droop?

## 10. Challenge Problem

An engineer tuning the platform keeps raising the proportional gain because both the settling time and the droop keep improving — until, near a gain of 0.4, the platform suddenly begins to oscillate violently. Explain what the engineer hit, why raising the gain helped right up until it didn't, and how the oil-spring resonance from Module 09 predicted this ceiling in advance. Then explain why, even at the best stable gain, a load that pushes the platform down leaves it holding slightly below target — and why no amount of proportional gain can drive that residual to exactly zero.

## 11. Common Mistakes

- **Maximising the gain.** Higher gain helps speed and droop only until the resonance takes over; there is a ceiling, set by the oil spring.
- **Expecting zero steady-state error.** Proportional control always leaves a droop under sustained load; it needs error to hold command.
- **Ignoring the plant's resonance when choosing gain.** The ~12 Hz oil spring sets the usable-gain limit; guessing invites oscillation.
- **Reading overshoot as harmless.** Growing overshoot signals the gain is exciting the resonance — a warning, not a cosmetic detail.

## 12. Key Takeaways

**The decision you can now make:** set a proportional gain that is fast and tight yet stays below the resonance — and recognise its residual droop.

- **Proportional control** commands in proportion to error: $u=K_p e$.
- Raising $K_p$ **shrinks both settling time** ($\tau=1/v_\text{max}K_p$) **and droop** ($e_\text{ss}=d/v_\text{max}K_p$) — both improve together.
- But $K_p$ is **bounded above by the oil-spring resonance** (~12 Hz): too high excites ringing, then oscillation.
- For the platform, **$K_p\approx0.1$–$0.15$** is the window: $\tau\approx0.08$ s, droop ~0.24 mm, well-damped.
- Proportional control **leaves a residual droop** it cannot erase. **Lesson 03 adds integral action to remove it.**

## AI Learning Companion

Copy a prompt into an AI assistant.

**Deepen** — the gain trade-off

```
Explain proportional control u = Kp*(x_target - x) for a hydraulic platform (v_max = 85 mm/s, ~12 Hz oil-spring resonance). Show how raising Kp shrinks both the settling time (tau = 1/(v_max*Kp)) and the steady-state droop (e_ss = d/(v_max*Kp)), and why the oil-spring resonance sets an upper limit on Kp. Use Kp = 0.15 as a worked design.
```

**Challenge** — the resonance ceiling

```
An engineer raises a hydraulic platform's proportional gain because settling time and droop keep improving, until near Kp = 0.4 it oscillates violently. Explain what ceiling was hit, how the ~12 Hz oil-spring resonance predicted it, and why - even at the best stable gain - a sustained load leaves a residual droop that no proportional gain can zero out.
```

**Explore** — proportional everywhere

```
Discuss why proportional control is the workhorse first stage of most controllers (motor drives, temperature loops, hydraulic servos), what its two fundamental limits are (resonance ceiling above, droop below), and when proportional-only is sufficient versus when the job demands integral or derivative action.
```

## Global Learning Support

Need this lesson in another language? Copy the prompt into an AI assistant. English remains the authoritative source.

**Supported languages (initial):** English · Español · 中文 (Simplified) · Türkçe

```
I just completed Module 10 Lesson 02 — Proportional control.
Explain this lesson in [Spanish / Simplified Chinese / Turkish], keeping common engineering terms in English where usual.
Then give me: a short summary, three practice questions, and one challenge problem.
```

---

*Proportional control gives a fast, tight response with one knob — but always a little droop, and a resonance ceiling on the gain. Next: Lesson 03 — PID control, adding integral action to erase the droop and derivative action to tame the ring.*
