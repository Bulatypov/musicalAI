# Musical AI: Music Generation with Transformers

A Transformer-based AI system that generates musical sequences using the MAESTRO dataset. Focused on capturing long-range dependencies for improved musical coherence.

---

## Features
- **Context-Aware Generation**: Leverages transformer architecture for better long-term pattern recognition
- **Meta Control**: Interactive parameters for temperature (0.1-2.0), note count (25-1000), tempo (0.1-2x), pitch modulation (0-127), and velocity (0-127)
- **MIDI Pipeline**: Full integration with MIDI input/output formats
- **Music Theory Compliance**: Maintains temporal consistency and scale relationships
- **Transformer Architecture**: 4-layer transformer encoder with positional encoding

**Current Capabilities**:  
`Produces monophonic piano sequences with improved temporal consistency compared to RNN approaches`

---

## Dataset
**MAESTRO Dataset** (MIDI and Audio Edited for Synchronous TRacks and Organization):
- 200+ hours of virtuoso piano performances
- Precision-aligned MIDI and audio recordings
- Preprocessed to 25-note sequences with 3D tensor format `(pitch, step, duration)`

---

## Architecture Overview

1. **Input Processing**
   - MIDI Tokenization:
     - 128-value pitch range `(0-127)`
     - Relative timing via `step` and `duration` parameters
   - Sequence Format:
     - Fixed-length inputs (25 notes)
     - Normalized values for model stability

2. **Transformer Architecture**
   - Input Embedding: `Dense(128)` + positional encoding
   - Core Structure:
     - 4 Transformer Encoder Layers
     - 8-Head Attention Mechanism
     - Feed-Forward Dimension: 512
   - Output Heads:
     - Pitch: `Dense(128)` with categorical cross-entropy
     - Step: `Dense(1)` with MSE + positive pressure
     - Duration: `Dense(1)` with MSE + positive pressure
   - Loss Weights:
     - Pitch: 0.05
     - Step: 1.0
     - Duration: 1.0

3. **Training Configuration**
   - Optimizer: Adam (`lr=0.005`)
   - Regularization:
     - Dropout (0.1)
     - Early Stopping (5 patience)
     - Model Checkpointing
   - Data Pipeline:
     - 20-file training subset
     - Cached shuffling with buffer size = dataset size

4. **Generation Process**
   - Seed Handling: First 25 notes from reference MIDI
   - Autoregressive Prediction:
     - 300-note default sequence length
     - Temperature-controlled sampling (0.1-5.0)
   - Post-Processing:
     - Tempo scaling (0.1-2.0x)
     - Pitch shifting (0-127)
     - Velocity adjustment (0-127)

5. **Current Limitations**
   - Instrumentation: Piano-only output
   - Polyphony: Single-voice generation
   - Rhythm Constraints:
     - Limited complex rhythmic patterns
     - Quantized timing resolution
   - Context Window:
     - 25-note input limitation
     - No explicit structural markers
   - Training Data:
     - Classical piano bias
     - No style transfer capabilities
   - Computational:
     - Colab-based training constraints

---

## Development Roadmap

1. **Polyphonic Expansion**  
   `Support multiple concurrent voices/notes`

2. **Multi-Instrument Synthesis**  
   `Enable orchestral composition through MIDI channel routing`

3. **Structural Conditioning**  
   `Implement verse/chorus/bridge section markers`

4. **Style Transfer**  
   `Add genre/era/composer conditioning parameters`

---

## Technical Notes
- **Interactive Elements**: Requires `ipywidgets` for web interface (Jupyter/Colab)
- **Rendering Issues**: Clear output cells before committing to GitHub ([related discussion](https://github.com/orgs/community/discussions/155944))
- **MIDI Playback**: Uses `fluidsynth` with `pretty_midi` interface

---

**Project Team**:  
Bulat Latypov, Timofey Ivlenkov  
Innopolis University, 2025
