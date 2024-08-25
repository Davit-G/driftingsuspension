# driftingsuspension

Ideation for an ethereal / nostalgic / texturing plugin

A flexible plugin which focuses on processing noise signals into a phase distortion to give more glued / natural texturing to the input signal

- Noise texturing section with layers
  - 2D XY Grid that contains "points" of dimension reductions of different audio samples in an XY space
  - Train a neural network that runs on different types of noise samples and generate unique noise textures live, the input vectors for them will be the 2d positions on the XY space
    - Possibly provide your own noise samples live, and auto generate similar textures
  - Give an option to either flip between neural generated noise textures, or direct audio files
  - Maybe blend between the closest points in the XY plane to modulate through?
  - Multi sample noise layer blending
  - Examples:
    - Several types of cassette white noise hiss
    - Ground hum (50hz or 60hz)
    - Vinyl crackle
    - Car traffic
    - Waves of ocean
    - Underwater bubbly noises
    - PWM interference from spinning motor
    - Vintage gear analog noise / hiss
    - Leaves crunching foley
    - Electromagnetic Radiation from the atmosphere recorded by NASA / similar
    - Water being poured in the sink
    - Rain / Thunder / Weather activity
    - Loud Wind
    - Rainforest noises
    - Geological noise / Geophony / Geothermal movement
    - Space background radiation
    - Radio AM Interference
    - Radio EM Noise / interference when on wrong channel
    - Custom processed noise textures with various filters
    - Aquatic "submarine" with high end material type noises
  - Add custom sounds in folder, accept wavs
  - OR drag and drop noise layers
  - Receive sidechain input instead if desired
  - Sounds can be sent into the phase distortion or added alongside signal (or both)
  - Noise layer can be mono or stereo
  - Noise filtering and compression section to tame or enhance noise material
  - Sounds can be RMed with the original signal to make them "crunchy"
  - Sounds can also be volume tracked, increasing / decreasing volume like the RC20 follower
- Phase Distortion with generated noise as the modulator to provide more detailed 'erosion' texture
- Phase distortion also provides options for wow - flutter pitch bend artifacts
  - Varying functions like perlin / S&H / Lagrange for different kinds of pitch bend artifacts
  - Stereo option to provide chorusing effect
  - Chorusing with stereo noise as well?
- Light Waveshaping Distortion / Clipping as a postprocessing effect to tame transient material
- Soft clipping on noise layer?????

- GUI should be dynamic / drifting
- Different colour palettes and patterns for different presets
- Interpolate between different plugin options for the visuals which could be rendered with raw glsl or be a 3d rendered thing
- I think a really cool dynamic environment would be really pretty but make the effort of the plugin 1000x


https://youtu.be/IET33sxcaRY
