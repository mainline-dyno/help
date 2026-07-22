---
title: "Math Functions"
description: "How to configure the real time math processing system"
---

## Settings

### Evaluation

 * **Continuous** — expressions are evaluated constantly at 100 Hz
 * **On Event** — expressions are evaluated only when the assigned event occurs (uses a fixed 10 ms `timeDelta`)

### Variable Name

This is the variable name that will become available for use in the expression. Names must match `[a-zA-Z_][a-zA-Z0-9_]*`.

### Derivative

When checked, an additional variable will become available containing the derivative of the input channel. The derivative variable uses the standard variable name with a `d` prefix.

e.g. If the channel variable is `EngineSpeed`, then the derivative variable will be `dEngineSpeed`.

The derivative is computed from consecutive channel timestamps when the delta is at least 10 ms. If derivative is disabled, `d{Name}` is always `0`.

### Reset totalTime on

Optional event channel (Continuous mode only). When the event fires, `totalTime` is reset to `0` on the next evaluation so `gen.*` waveforms can restart without recompiling. This does **not** clear filter or integrator state.

Changing the expression text triggers recompile and a full state reset (`totalTime`, filters, integrators).

## Expression Syntax

Expressions are compiled once, then evaluated each tick to produce a single numeric output channel.

### Numeric Literals

| Type    | Examples                              |
|---------|---------------------------------------|
| Integer | `42`, `0x2A` (hex), `0b1010` (binary) |
| Float   | `0.5`, `.5`, `1e-3`, `2.5E+2`         |
| Boolean | `true`, `false` (stored as `1` / `0`) |

Line comments are supported: `// everything after is ignored`

### Constants

| Name | Value              |
|------|--------------------|
| `PI` | π                  |
| `E`  | e (Euler's number) |

`math.PI` is also available as an alias for `PI`.

### Arithmetic Operators

| Operator    | Description         | Example        |
|-------------|---------------------|----------------|
| `+`         | Addition            | `rpm + 500`    |
| `-`         | Subtraction         | `temp - 20`    |
| `*`         | Multiplication      | `load * 0.5`   |
| `/`         | Division            | `fuel / 4`     |
| `%`         | Modulus (remainder) | `counter % 10` |
| `^` or `**` | Exponent            | `x ^ 2`        |

### Comparison Operators

Returns `1` (true) or `0` (false).

| Operator | Description              |
|----------|--------------------------|
| `==`     | Equal to                 |
| `!=`     | Not equal to             |
| `<`      | Less than                |
| `>`      | Greater than             |
| `<=`     | Less than or equal to    |
| `>=`     | Greater than or equal to |

### Logical Operators

| Operator       | Description |
|----------------|-------------|
| `&&` or `and`  | Logical AND |
| `\|\|` or `or` | Logical OR  |
| `!` or `not`   | Logical NOT |

Logical operators short-circuit: in `a && b`, `b` is not evaluated if `a` is falsy; in `a || b`, `b` is not evaluated if `a` is truthy.

A value is **truthy** when it is not `0` and not `NaN`. `NaN` is falsy — use `util.IfNaN` or `math.IsNaN` when you need explicit handling.

### Conditional (Ternary) Operator

```
condition ? valueIfTrue : valueIfFalse
```

Example: `rpm > 3000 ? 1 : 0`

### Local Variables

Use `let` to declare intermediate values within an expression. Separate statements with `;`.

```
let x = rpm / 60;
let y = x * 2;
y + offset
```

The last expression in the `let` body is the result.

## Built-in Variables

| Name             | Description |
|------------------|-------------|
| `timeDelta`      | Time since last evaluation of this expression, in seconds. In continuous mode this is the backend tick interval. In event mode it is `0.01` s. |
| `totalTime`      | Accumulated time since the first evaluation after compile (or since the last **Reset totalTime** event). Used by `gen.*` and `util.DistanceDelta`. |
| `previousResult` | Output value from the previous evaluation of this expression. `0` on the first eval after compile. |

## Stateful Evaluation

Functions in the `filter`, `calc`, and `gen` namespaces maintain state across evaluations. Each **call site** gets its own independent state slot — two `filter.Exponential` calls in the same expression do not share memory:

```
filter.Exponential(A, 0.1) + filter.Exponential(B, 0.2)
```

Several filters seed their internal state from the first sample on the first tick, so the first output may equal the first input rather than a neutral value.

## Math Utilities

All trig functions are in radians.

| Function | Description |
|----------|-------------|
| `math.Sin(x)` | Sine of x |
| `math.Cos(x)` | Cosine of x |
| `math.Tan(x)` | Tangent of x |
| `math.Asin(x)` | Arcsine of x |
| `math.Acos(x)` | Arccosine of x |
| `math.Atan(x)` | Arctangent of x |
| `math.Atan2(y, x)` | Quadrant-correct arctangent |
| `math.Hypot(p, q)` | Hypotenuse of p and q |
| `math.Exp(x)` | Base e exponential of x |
| `math.Abs(x)` | Absolute value of x |
| `math.Log(x)` | Natural log of x |
| `math.Log10(x)` | Base-10 log of x |
| `math.Sqrt(x)` | Square root |
| `math.Min(x, y)` | Minimum |
| `math.Max(x, y)` | Maximum |
| `math.Clamp(x, min, max)` | Clamps x between min and max |
| `math.Pow(x, y)` | x to the power of y |
| `math.Pow10(n)` | 10 to the power of n |
| `math.Round(x)` | Rounds to the nearest integer |
| `math.Floor(x)` | Rounds toward −∞ |
| `math.Ceil(x)` | Rounds toward +∞ |
| `math.Trunc(x)` | Rounds toward zero |
| `math.Sign(x)` | Returns −1, 0, or +1 |
| `math.Mod(x, y)` | Floating-point remainder |
| `math.Deg2Rad(x)` | Converts degrees to radians |
| `math.Rad2Deg(x)` | Converts radians to degrees |
| `math.Blend(t, x, y)` | Linear blend: `(1−t)·x + t·y`, with t clamped to [0, 1] |
| `math.Lerp(t, x, y)` | Same as `math.Blend` |
| `math.Sq(x)` | x² |
| `math.IsNaN(x)` | Returns `1` if x is NaN, else `0` |
| `math.IsInf(x)` | Returns `1` if x is ±Inf, else `0` |

## Filter Utilities

| Function | Description |
|----------|-------------|
| `filter.Exponential(input, alpha)` | Exponential moving average. `alpha` is the smoothing factor (0.001–0.999). |
| `filter.MovingAverage(input, n)` | Moving average over the last n samples. n must be constant (clamped to 1–1024). |
| `filter.Median(input, n)` | Moving median over the last n samples. n should be an odd number. |
| `filter.RateOfChange(input, maxPositiveRatePerSecond, maxNegativeRatePerSecond)` | Limits the rate of change of the input. Both rate limits are positive numbers. |
| `filter.Schmitt(input, lowValue, highValue)` | Schmitt trigger. Returns `1` when above `highValue`, `0` when below `lowValue`. |
| `filter.DelayedSchmitt(input, lowValue, highValue, offTimeS, onTimeS)` | Schmitt trigger with time delays in seconds. |
| `filter.LowPass(input, cutoffHz)` | First-order low-pass filter. `cutoffHz` is the −3 dB corner frequency. |
| `filter.Deadband(input, width)` | Ignores changes smaller than `width`. |
| `filter.Ramp(input, maxRate)` | Limits output rate of change to `maxRate` units per second. |
| `filter.PeakHold(input, decayRate)` | Tracks the maximum value seen, decaying at `decayRate` units per second. |
| `filter.Previous(input)` | Returns the previous input sample. |
| `filter.HoldWhile(valid, input)` | Passes `input` through while `valid` is truthy; otherwise returns the last held value. |
| `filter.ExponentialTau(input, tauSeconds)` | EMA with time constant in seconds. |
| `filter.WindowMin(input, n)` | Minimum over the last n samples. |
| `filter.WindowMax(input, n)` | Maximum over the last n samples. |

## Calculus Utilities

| Function | Description |
|----------|-------------|
| `calc.Integrate(input, deltaTime, initial, enable)` | Running sum. When `enable` is truthy, accumulates `input × deltaTime`. When falsy, resets to `initial`. Typically `deltaTime` is `timeDelta`. |
| `calc.IntegrateClamped(input, deltaTime, initial, min, max, enable)` | Same as `calc.Integrate`, but clamps the output between min and max. |
| `calc.Differentiate(input)` | Numerical derivative: `(input − previous) / timeDelta`. |
| `calc.Timer(run, reset)` | Elapsed-time timer in seconds. Increments while `run` is truthy; resets when `reset` is truthy. |
| `calc.Latch(value, set, reset)` | Sample-and-hold latch. Captures `value` when `set` is truthy; clears when `reset` is truthy. |
| `calc.Rising(condition)` | Returns `1` for one tick on a rising edge of `condition`, else `0`. |
| `calc.Falling(condition)` | Returns `1` for one tick on a falling edge of `condition`, else `0`. |
| `calc.EdgeCount(condition, reset)` | Counts rising edges of `condition`. Resets to `0` when `reset` is truthy. |

## Utility Functions

| Function | Description |
|----------|-------------|
| `util.IfNaN(value, fallback)` | Returns `fallback` if `value` is NaN, else `value`. |
| `util.IfInf(value, fallback)` | Returns `fallback` if `value` is ±Inf, else `value`. |
| `util.SafeDiv(numerator, denominator, fallback)` | Returns `numerator / denominator`, or `fallback` if `denominator == 0`. |
| `util.Scale(value, inMin, inMax, outMin, outMax)` | Linear map from `[inMin, inMax]` to `[outMin, outMax]`. |
| `util.Within(value, lo, hi)` | Returns `1` if `lo ≤ value ≤ hi`, else `0`. |
| `util.Coalesce(a, b, …)` | Returns the first argument that is not NaN (2–8 arguments). |
| `util.PercentError(measured, target)` | `((measured − target) / target) × 100`. |
| `util.Piecewise(x, x0, y0, x1, y1, …)` | Piecewise-linear lookup table (up to 6 knots). |
| `util.DistanceDelta(speedMps)` | Returns `speedMps × timeDelta` — distance travelled over the last tick. |
| `util.KphToMps(kph)` | Converts km/h to m/s. |
| `util.MpsToKph(mps)` | Converts m/s to km/h. |

## Signal Generators

Timed waveform generators. All use `totalTime` (seconds since first eval after compile or last reset event).

| Function | Description |
|----------|-------------|
| `gen.Sine(frequencyHz, amplitude, offset)` | Sine wave |
| `gen.Cosine(frequencyHz, amplitude, offset)` | Cosine wave |
| `gen.Square(frequencyHz, amplitude, offset)` | Square wave (50% duty) |
| `gen.Square(frequencyHz, amplitude, offset, duty)` | Square wave with configurable duty cycle |
| `gen.Triangle(frequencyHz, amplitude, offset)` | Triangle wave |
| `gen.Sawtooth(frequencyHz, amplitude, offset)` | Rising sawtooth |
| `gen.Step(timeSeconds, valueBefore, valueAfter)` | Step function at `timeSeconds` |
| `gen.Pulse(periodSeconds, amplitude, offset, pulseWidthSeconds)` | Periodic pulse |
| `gen.Ramp(slope, min, max)` | Ramp `slope × totalTime`, clamped to `[min, max]` |
| `gen.Noise(amplitude, offset, seed)` | Deterministic pseudo-random noise |
| `gen.Chirp(startHz, endHz, durationSeconds, amplitude)` | Linear frequency chirp |

Example — smoothed test signal:

```
filter.LowPass(gen.Sine(1, 100, 0), 0.2)
```
