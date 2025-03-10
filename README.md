# Musical AI: Text-to-Music Generation with Transformers

A transformer-based AI system that generates monophonic musical sequences from text prompts (e.g., "classical piano fast tempo"). Currently focused on single-instrument generation using the MAESTRO dataset.

---

## Features
- **Text-to-Music Generation**: Convert natural language descriptions into musical sequences.
- **Tempo Control**: Generates music matching specified tempos ("slow", "moderate", "fast").
- **MIDI Integration**: Outputs playable MIDI files.
- **Theory-Compliant**: Respects musical fundamentals (tempo, note duration, scales).
- **Transformer Architecture**: Leverages GPT-style models with text conditioning.

**Current Stage**:  
`AI can produce monophonic templates of music (using only one instrument)`

---

## Installation
```bash
git clone https://github.com/your_username/musical-ai.git
cd musical-ai
```

**Dependencies**:
- `torch>=2.0.0`
- `transformers>=4.30.0`
- `pretty_midi>=0.2.9`
- `pandas>=1.5.0`

---

## Dataset
We use the **MAESTRO Dataset** (MIDI and Audio Edited for Synchronous TRacks and Organization):
- 200+ hours of classical piano performances
- Tempo annotations extracted from MIDI metadata
- Processed to include quantized note durations (0.25s, 0.5s, 1.0s, 2.0s)

---

## Model Architecture

Key components:
1. **Text Encoder**: Mini-GPT2 (4 layers, 256-dim embeddings)
2. **Music Embeddings**: 128 MIDI pitches + 4 duration tokens + 1 [PAD]
3. **Transformer**: 8-head attention, 512-dim embeddings
4. **Conditioning**: Text features projected into music embedding space


## Limitations (Current Stage)
- Monophonic output only (single instrument)
- Fixed 0.5s note duration quantization
- Limited to classical piano (MAESTRO domain)
- No lyrics support yet

---

## Future Roadmap
1. **Polyphonic Generation** (Stage 3):  
   `AI can produce polyphonic templates of music`
2. **Lyrics Integration** (Stage 4):  
   `AI can sing lyrics`
3. **Multi-Instrument Support**

---

**Contributors**:  
Bulat Latypov, Timofey Ivlenkov  
Innopolis University, 2024
