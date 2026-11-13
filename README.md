# Vinyl phase / tracking-angle analyser

Streamlit app that measures the channel phase of a test-record tone, converts it to horizontal tracking angle
(angle = arctan(v * dt / W)), and fits the tonearm geometry (overhang, offset angle, zenith offset).

## Run

    pip install -r requirements.txt
    streamlit run app.py

On Linux the `soundfile` package needs libsndfile (installed by the pip wheel; on Streamlit Community Cloud add
a `packages.txt` containing `libsndfile1` if loading WAV files fails).

## Notes

- Upload limit is 500 MB (`.streamlit/config.toml`); the analysis reads the file in blocks (about 200 MB peak
  memory on a 255 MB WAV). FLAC is about half the size and uploads faster.
- `streamlit==1.40.2` is pinned for old Safari/iPad browsers; the app also runs on current Streamlit.
- Supports Sperling and Clearaudio test-record profiles; add more in `RECORD_PROFILES` in `app.py`.
- Stylus contact width W is calibrated at 18.46 um at 127 mm (growth exponent q = 0.41) from a 3 degree cartridge
  twist on the Clearaudio 3150 Hz track. Change both in the sidebar for another stylus.
- The in-app *User guide* explains every setting.
