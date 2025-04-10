# Musical AI: Music Generation with RNN

An RNN AI system that generates monophonic musical sequences from input samples and meta-data. Currently focused on single-instrument generation using the MAESTRO dataset.

---

## Features
- **Music Generation**: Generate music based on training data and input sample.
- **Meta Control**: Generates music matching specified meta information: temperature, number of notes, tempo, pitch and velocity of the music.
- **MIDI Integration**: Outputs playable MIDI files.
- **Theory-Compliant**: Respects musical fundamentals (tempo, note duration, scales).
- **RNN Architecture**: Uses RNN architecture to predict the next note.

**Current Stage**:  
`AI can produce monophonic templates of music (using only one instrument)`

---

## Dataset
We use the **MAESTRO Dataset** (MIDI and Audio Edited for Synchronous TRacks and Organization):
- 200+ hours of classical piano performances
- Tempo annotations extracted from MIDI metadata
- Processed to include quantized note durations (0.25s, 0.5s, 1.0s, 2.0s)

---

## Project Architecture

1. Input Representation
   * Midi Tokenization:
     - Pitch of 128 possible values `(0-127)`.
     - Step: time before next note.
     - Duration: duration of the note.
   * Sequences:
     - Fixed length of time steps.
     - Format: tensor with 3 keys `(pitch, step, duration)`.
2. Model Architecture
   * Input Layer: takes sequences of 25 notes.
   * LSTM Layer: 128 neurons with `tanh` activation and sensitive time dependencies between notes.
   * Output Heads:
     - Pitch Prediction: `Dense(128, activation="linear", name="pitch")`.
     - Step Prediction: `Dense(1, activation="linear")` with `mse_with_positive_pressure`.
     - Duration Prediction: `Dense(1, activation="linear")` with `mse_with_positive_pressure`.
   * Loss Weighting:
     - Pitch: 0.05
     - Step: 1.0
     - Duration: 1.0
3. Training Pipeline
   * Optimizator: Adam with `learning rate = 0.005`
   * Regularization:
     - Early Stopping
     - Checkpointing
   * Data:
     - Training on `num_files` (in example - `5`) files from MAESTRO
     - Augmentation: Shuffling + caching
4. Music Generation
   * Input sample: first 25 notes from sample file
   * Output size: interactive parameter of the number of notes (by default - 300), the note can have zero duration, so the model precits not only notes, but spaces too.
   * Autoregressive Prediction: cyclic prediction of the next notes with interactive temperature (by default - 1.0).
   * Post-processing: changing `tempo` (0.1-2.0), `pitch` (0-127) and `velocity` (0-127) of the notes.
5. Limitations (For now):
   * Generation only for one instrument.
   * No accords understanding.
   * Timing:
     - All of the notes quantazing with fixed timing (based on `tempo`)
     - Lack of difficult rhytmic patterns (like triols)
   * Context limit:
     - LSTM usage limits long context
     - No explicit structure modelling (couplet/chorus)
   * Data:
     - Trained only on MAESTRO (only piano classic)
     - No generation conditions (like `sad fast tempo jazz`)
   * Resources: we are limited in recourses, used only Google Colab.

---

## Future Roadmap
1. **Polyphonic Generation**:  
   `AI can produce polyphonic templates of music` 
2. **Multi-Instrument Support**:   
   `AI can produce templates of music containing different instuments` - This stage is important, as MIDI provides great and easy approach of generating snippets of different instruments and combining them
3. **Transformer Approach**:   
   `Transformers can be better for global context`


---
## Problems
According to the [discussion](https://github.com/orgs/community/discussions/155944) and [issue](https://github.com/googlecolab/colabtools/issues/5255), with recent updates GitHub has problems with displaying Jupyter Notebooks with Interactive Widgets (ipywidgets). One of possible fixes - clearing output of code snippets containing such widgets

---
**Contributors**:  
Bulat Latypov, Timofey Ivlenkov  
Innopolis University, 2025
