!!! abstract "You are here"
    **Module 08 — Electrohydraulic Control**  ·  **Unit 3 — The Complete Interface**  ·  **Lesson 05 — The driven-and-sensed platform**

# Lesson 05 — The driven-and-sensed platform

> **Module 08 · Lesson 05** · *Command in, measurements out.*
> Command and sensing exist separately. This lesson assembles them into one interface — a platform you can drive with a signal and read back through sensors — and closes Module 08, ready for simulation and control.
>
> **Learning outcome:** Describe the platform as an electrohydraulic interface — one command input and two measurement outputs — state each signal's range and rate, and explain why the loop is still open and what closing it requires.

---

## 1. Why This Matters

Across this module the platform gained an electrical command and two electrical senses, but they have been separate ideas — a wire in, a couple of wires out, never drawn as one thing. Assembling them is what turns a pile of components into a *system a computer can work with*. Once the platform presents a clean interface — write a command, read a position and a pressure — it stops being hydraulics-with-electronics and becomes a **plant**: a box with defined inputs and outputs that a controller, or a simulator, can reason about without knowing a single thing about spools or seals. Every automatic thing the platform will ever do runs through this interface.

So the decision this lesson makes is: **what exactly is the platform's interface — which signals go in, which come out, and with what range and rate?** One command goes in: the proportional-valve signal that sets speed. Two measurements come out: position from the in-bore sensor and pressure from the cap transducer, the latter also giving load. That is the whole boundary — command in, measurements out. And naming it exposes the one thing still missing: nothing yet connects the measurements *back* to the command. The platform can be driven and it can be read, but it cannot yet correct itself. The interface is complete; the *loop* is still open — and closing it is the job of Module 10.

## 2. Physical Intuition

Draw a box around the whole platform — pump, valve, cylinder, load, sensors — and look only at the wires crossing the boundary. Going in: one signal, the command, which through everything you built in Lessons 01 and 02 sets the platform's speed. Coming out: two signals, the position (where the platform is, to a fraction of a millimetre) and the cap pressure (how hard it is pushing, which is the load). Inside the box is all the hydraulics; outside, a controller sees only those three wires. That abstraction is powerful — it means the platform can be handed to someone who thinks purely in signals, and they can drive it and watch it respond without touching oil.

Now trace a motion through the interface and feel what is missing. Command 50% and the platform rises at ~42 mm/s; the position output climbs, the pressure output holds at the load value. Command zero and it stops — wherever it happens to be. To land on a target you would command some speed for some time and hope, because nothing is *watching the position and easing off as it approaches*. That watching-and-adjusting is the closed loop, and it is deliberately not here yet. What this lesson delivers is the thing the loop will wrap around: a platform that faithfully turns a command into motion and reports its state honestly. Give that to a simulator and it can predict the motion; give it to a controller and it can finally close the gap between command and result.

## 3. The Idea You Now Need

The platform is now a **plant** — a block with one input and two outputs:

$$ u \;\longrightarrow\; \boxed{\text{platform}} \;\longrightarrow\; (x,\ p) $$

The input is the command $u\in[0,1]$; the outputs are position $x$ and cap pressure $p$. The relationships you have built define the block: command sets speed, speed integrates to position, and pressure reports load:

$$ v = 85\,u\ \text{mm/s}, \qquad x(t)=\int v\,dt, \qquad \text{load}=\frac{p\,A_\text{cap}}{g} $$

Each signal has a spec: command $u$ at ~1 kHz; position $x$ over 0–600 mm, ≥14-bit (≤0.037 mm), ~1 kHz; pressure $p$ over 0–250 bar, 0.5% FS. Open-loop, a move is just $x=85\,u\,t$ — command a speed for a time. But that integral has no correction in it, so any drift in the command-to-speed map (temperature, load) accumulates as position error. Closing the loop means computing the command *from* the measured position and a target,

$$ u = f(x_\text{target}-x), $$

so the platform trims its own error. Building that $f$ is Module 10; this lesson guarantees the $u$, $x$, and $p$ it needs all exist and are good enough.

## 4. Visual Explanation

<figure markdown>
  ![A block diagram of the driven-and-sensed platform. On the left, a command signal u enters a block labelled platform, which contains the proportional valve, cylinder, and load. Out of the block come two measurement signals: position x from the in-bore sensor (0 to 600 mm, 14-bit or better, ~1 kHz) and cap pressure p from the transducer (0 to 250 bar, 0.5% full scale), with pressure also giving load through force equals pressure times area. A dashed, greyed feedback path loops the measurements back toward the command with a label saying closed in Module 10, showing the loop is currently open. A side table lists the three signals with their range and rate.](assets/m08-l5-interface.svg){ width="720" }
</figure>

Read the solid path first: **command in → platform → position and pressure out**. That solid path is what this module delivers — a plant you can drive and read. Then note the **dashed feedback path**, greyed and labelled "closed in Module 10": the wire that would carry the measurements back to compute the command does not exist yet. That single missing connection is the difference between *driven-and-sensed* (this module) and *controlled* (Module 10). The side table is the contract: command at 1 kHz, position 0–600 mm to better than 0.04 mm, pressure 0–250 bar. Anyone building a simulator or a controller works to that table and ignores everything inside the box.

## 5. Engineering Example

This interface — a command input and a set of measurement outputs — is exactly how real motion systems are packaged and sold. A hydraulic actuator with an integrated valve and position sensor ships as a unit with a defined electrical interface: a command line and feedback lines, documented by range and update rate, ready to wire to any controller. The machine builder does not re-derive the hydraulics; they read the datasheet's interface and design their control and automation against it. The same abstraction lets a controls engineer develop and test on a *simulated* plant with the identical interface, then drop in the real one. Defining the boundary cleanly — these signals, these ranges, these rates — is what makes hydraulic hardware, simulation models, and control software interchangeable pieces rather than one tangled whole. It is the reason a team can build the machine, the model, and the controller in parallel.

## 6. Worked Example

<div class="worked" markdown="1">

**Given** — the platform interface: command $u$; outputs position $x$ (0–600 mm) and cap pressure $p$ (0–250 bar). Full speed 85 mm/s.

**Find** — the open-loop result of commanding 40% for 2 s, the load at 100 bar, and what the loop needs to hold a target.

**Assumptions**

- Command-to-speed is $v=85u$; no feedback (open loop).

**Solution**

$$ v = 85\times0.40 = 34\ \text{mm/s}, \qquad \Delta x = v\,t = 34\times2 = 68\ \text{mm} $$
$$ \text{load} = \frac{p\,A_\text{cap}}{g} = \frac{100\ \text{bar}\times1963.5\ \text{mm}^2}{9.81} \approx 2000\ \text{kg} $$
$$ \text{to hold a target: } u = f(x_\text{target}-x)\ \text{(needs the measured } x\text{)} $$

**Result**

$$ \boxed{\text{Open loop: }68\text{ mm move, load } \approx 2000\text{ kg; holding a target needs feedback from } x} $$

**Engineering Interpretation** — The interface does two of the three things a controlled platform needs: it **acts** (68 mm from a timed command) and it **senses** (position to <0.04 mm, load to ±25 kg). What it does not do is **decide** — nothing uses the measured $x$ to adjust the command, so the 68 mm move is only as accurate as the command-to-speed map, which drifts. That is the deliberate boundary of this module: a plant that is fully driven and fully sensed but not yet self-correcting. The moment you connect the position output back to the command input through a rule $f$, the platform gains the third ability and can hold ±1 mm despite drift — the closed loop of Module 10. And because the interface is clean, Module 09 can first build a *simulation* with the same $u \to (x,p)$ boundary and predict the motion before any steel is cut. The value delivered here is not a new capability but a well-defined one: a platform reduced to three trustworthy signals.

</div>

## 7. Interactive Demonstration

<iframe src="demos/lesson05_interface.html" title="The driven-and-sensed platform — command in, measurements out" style="width:100%;height:900px;border:1px solid #e2e8f0;border-radius:12px"></iframe>

Command a speed and run the clock: watch the position output climb and the pressure output report the load — the platform driven and sensed through one interface. Try to land on the target by command and timing alone, and see why, with the feedback path still open, you cannot reliably hold it.

## 8. Coding Exercise

```python
A_cap, v_max, stroke = 1963.5, 85.0, 600.0

class Platform:                       # the plant: command in, (x, p) out
    def __init__(self, load_kg): self.x=0.0; self.load=load_kg
    def step(self, u, dt):            # open loop: command u for dt seconds
        self.x = min(self.x + v_max*u*dt, stroke)
        return self.x, self.pressure()
    def pressure(self):               # cap pressure reports the load
        return self.load*9.81/(A_cap*1e-6)/1e5

p = Platform(2000)
x, p_bar = p.step(0.40, 2.0)
print(f"{x:.0f} mm, {p_bar:.0f} bar")   # -> 68 mm, ~100 bar

# open loop cannot hold a target: nothing reads x to adjust u
target = 300
# u = controller(target - p.x)        # <-- the missing piece: Module 10
```

**Your task:** run several `step` calls and confirm position accumulates as 85·u·t. Then answer in a comment: which single line would you add — and what would it read — to turn this open interface into a closed loop?

## 9. Knowledge Check

Formative — unlimited attempts, immediate feedback; does not affect your grade.

<iframe src="quizzes/lesson05_quiz.html" title="The driven-and-sensed platform — knowledge check" style="width:100%;height:900px;border:1px solid #e2e8f0;border-radius:12px"></iframe>

1. What is the platform's interface — which signal goes in, which come out?
2. Why is treating the platform as a "plant" with defined inputs and outputs useful?
3. Open-loop, how far does commanding 40% for 2 s move the platform, and what sets the accuracy?
4. What is still missing that keeps the loop open, and what would close it?
5. What does this interface hand off to Modules 09 and 10?

## 10. Challenge Problem

Two engineers receive the platform as an interface: a command input and position/pressure outputs, documented by range and rate. One is asked to build a simulation of the platform's motion; the other to build a controller that holds ±1 mm. Explain how the *same* interface serves both tasks, what each engineer does with the three signals, and why defining the boundary cleanly lets them work in parallel without knowing each other's internals. Then explain what neither can do until the loop is closed, and which of them closes it.

## 11. Common Mistakes

- **Thinking driven-and-sensed means controlled.** All three signals can exist while the loop stays open; control needs the feedback *connection*, not just the wires.
- **Reaching a position by command and timing.** Open-loop $x=85ut$ drifts with temperature and load; only feedback holds a target.
- **Ignoring the signal specs.** The interface is only as good as its ranges and rates; a coarse or slow output breaks the loop built on it.
- **Burying the interface in the hydraulics.** The point is the abstraction — three signals a controller can use without knowing the internals.

## 12. Key Takeaways

**The decision you can now make:** describe the platform as an electrohydraulic interface — one command in, position and pressure out — and say what closing the loop requires.

- The platform is now a **plant**: command $u$ in; position $x$ and pressure $p$ out.
- **Command** sets speed ($v=85u$); **position** reports where (0–600 mm, ≥14-bit); **pressure** reports load ($p\,A/g$, 0–250 bar).
- Open-loop a move is $x=85\,u\,t$ — accurate only as far as the command map holds; it **drifts**.
- The **loop is still open**: nothing feeds the measured $x$ back to the command. Closing it needs a rule $u=f(x_\text{target}-x)$.

**Module 08 complete.** The platform is driven and sensed — a clean interface of command in, measurements out. **Module 09 simulates** this plant to predict its motion before building; **Module 10 closes the loop** to hold ±1 mm.

## AI Learning Companion

Copy a prompt into an AI assistant.

**Deepen** — the plant abstraction

```
Explain how an electrohydraulic lift platform becomes a "plant" with one command input (a proportional-valve signal setting speed, v = 85*u mm/s) and two measurement outputs (position 0-600 mm and cap pressure 0-250 bar, pressure giving load). Why is this input/output abstraction the foundation for both simulation and control?
```

**Challenge** — one interface, two jobs

```
The same platform interface (command in; position and pressure out) is handed to one engineer to simulate the platform and another to build a ±1 mm controller. Explain how each uses the three signals, why a clean boundary lets them work in parallel, and what neither can achieve until the feedback loop is closed.
```

**Explore** — open vs closed loop

```
Contrast open-loop and closed-loop operation of a driven-and-sensed hydraulic platform. Open-loop, position is x = 85*u*t from a timed command; closed-loop, the command is computed from the measured position and a target. Explain why open-loop drifts and what the feedback connection adds.
```

## Global Learning Support

Need this lesson in another language? Copy the prompt into an AI assistant. English remains the authoritative source.

**Supported languages (initial):** English · Español · 中文 (Simplified) · Türkçe

```
I just completed Module 08 Lesson 05 — The driven-and-sensed platform.
Explain this lesson in [Spanish / Simplified Chinese / Turkish], keeping common engineering terms in English where usual.
Then give me: a short summary, three practice questions, and one challenge problem.
```

---

*Module 08 is complete — the platform is a clean interface of command in, measurements out. Next: Module 09 — Modeling and Simulation, where we build a model of this plant to predict its motion before committing to hardware.*
