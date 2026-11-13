# New-user guide

Vinyl phase / tracking-angle analyser (script rev R15-web4)

## 1. What the app does

You upload a stereo needle drop of a single-tone test record (Sperling 1000 Hz or Clearaudio 3150 Hz). The app:

1. tracks the tone and measures the **R-L phase difference** over the whole band,
2. converts time to **groove radius** from the record profile,
3. converts phase to the **horizontal tracking angle** in degrees, so the result can be compared directly with an HTA test record (for example CBS STR-112),
4. fits the **tonearm geometry** (overhang, offset angle, zenith offset) and compares it with the ideal Loefgren A curve.

## 2. Quick start

1. Upload a stereo WAV or FLAC. The limit is 500 MB. FLAC is about half the size and uploads faster.
2. Choose the **Record**. The tone frequency fills in by itself.
3. Choose the **Tracking-error fit model**:
   - *fixed pivot-to-spindle*: enter your measured pivot-to-spindle distance.
   - *fixed effective length*: for adjustable-base arms such as an SME. Enter the effective length.
4. Press **Run analysis**.
5. Open **Show page 2** to see phase, tracking angle in degrees, the fitted curve, the ideal curve and the fit table.

Record the whole tone band and start recording before the needle drop.

## 3. How to read the plots

- **Blue**: your measurement.
- **White dashed**: the geometry curve fitted to your data.
- **Green dotted**: the ideal Loefgren A curve. It is pure theory, with no zenith error or other adjustment.
- **Yellow dash-dot** (optional): the ideal curve shifted by a constant zenith angle, so you can see what a zenith error does.
- **Red dashed vertical lines**: the record's marker radii (for Sperling, the IEC nulls at 66.0 and 120.9 mm).
- The upper plot is in **phase** (degrees of phase). The lower plot is the true **angular tracking error** (degrees of angle). The y-axes differ, so compare the shapes and the nulls, not the numbers between the two plots.
- The title's second line is the analysed file name. A light-blue note in the plot shows the zenith offset used.

## 4. Phase to angle: the W and q settings

The two groove walls are a distance W apart across the stylus contact. At a tracking angle α one wall is W·tan(α) ahead of the other along the groove, which gives a time delay between the channels:

    α = arctan(v · Δt / W)
    Δt = phase / (360 · f)
    v  = 2π · r · rpm / 60

Because W is not known directly, it is calibrated from the 3° cartridge twist on the Clearaudio 3150 Hz track:

    W(r) = W₀ · (127 / r)^q        W₀ = 18.46 µm at 127 mm,  q = 0.41

W₀ and q are empirical, effective values. Their physical cause is not established. Change them in the sidebar if you calibrate another stylus.

### Question: if I want a constant W, do I set q = 1 or 0?

**Set q = 0.** With q = 0 the factor (127/r)^q is 1 at every radius, so W stays at W₀ (18.46 µm, or any value you type).

q = 1 is not constant. It makes W grow as 1/r, so the contact width increases toward the centre of the record.

Nothing is fitted in either case. W₀ and q are values you enter. The fit only adjusts the arm geometry.

Keep in mind that the twist calibration gave q ≈ 0.41. With q = 0 the angles at the inner radii will come out different from the calibrated model.

## 5. Zenith offset and offset angle

- A **zenith offset** is a constant *angular* error from the cartridge or the record. The phase it causes changes with radius. It is not a constant phase.
- It is exactly degenerate with the arm's **offset angle**: a change of 1° in one cannot be told apart from 1° in the other. Fix one and fit the other:
  - enter a known zenith offset and fit the offset angle, or
  - tick *Offset angle is known* and fit the zenith offset.
- **Stylus tip zenith error** is an optional extra constant angle that is added to the zenith offset.
- A true constant phase offset, such as a channel delay in the electronics, is not modelled. A level that no plausible zenith error explains may be of this kind.

## 6. Other settings in short

| Setting | Meaning |
|---|---|
| Smoothing (ms) | Moving average over the curves. Default 18000. |
| Trim each end | Seconds removed before fitting. Keep it at least half the smoothing value, plus any silence at the start or end. |
| Mode | *peak* is for single-tone records. |
| Tone frequency | A wrong value makes the app track noise, which shows up as huge random phase values. |
| Loefgren R1 / R2 | Groove radii used for the ideal curve (defaults 60.3 and 146.05 mm). |

## 7. Troubleshooting

- **Phase in the thousands of degrees, or random wiggles**: the record or the tone frequency is wrong.
- **"Plotting vs TIME" warning**: the detected tone length does not match the profile (wrong record, partial recording or a large speed error).
- **"Tone not detected"**: raise *dB below 90th percentile* or shorten the hold time.
- **Fit does not follow the data**: check the smoothing, the trim, the x-axis limits and the fixed pivot-to-spindle or effective-length value.
- **Uploader still says 200 MB**: `.streamlit/config.toml` must be in the `.streamlit` folder next to `app.py` in the repo root. A hosted app must be rebooted after it is added. The footer shows the active limit.

*This app is experimental. Check the results against your own expectations before relying on them.*
