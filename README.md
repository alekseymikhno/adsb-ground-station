# ADS-B 1090 MHz Ground Station (PDX Airspace)

A 1090 MHz ADS-B receiver system tracking aircraft over Portland, OR, paired with an in-progress custom RF front end and an LLM-based anomaly-detection pipeline.

## Status

- **Pi 5 receiver**: Live and collecting flight data (May–July 2026)
- **Custom RF front end**: In design: 1090 MHz filter-LNA (TA1090EC SAW + PGA-103+). Schematic and QucsStudio simulation complete (~13.3 dB cascade gain, ~3.2 dB NF); not yet sent to fabrication.
- **Anomaly detection**: LLM analysis pipeline in development

## What It Does

1. **RF capture**: 1090 MHz antenna -> TA1090EC SAW filter -> PGA-103+ LNA -> Nooelec NESDR SMArt v5 SDR
2. **Decode**: Raspberry Pi 5 runs dump1090 to decode Mode S / ADS-B messages in real time
3. **Analysis**: LLM-based anomaly detector flags unusual flight patterns over PDX airspace
4. **Output**: Logged to SQLite, served via web dashboard with a Tar1090 UI

## Folder Structure

- `hardware/rf-frontend/` — KiCad schematic, PCB layout, component libraries
  - `kicad/` — schematic and board files
  - `simulation/` — QucsStudio S-parameter simulations and results
- `software/receiver/` — dump1090 config, startup scripts
- `software/anomaly-detection/` — analysis pipeline code
- `docs/` — datasheets, system diagrams, build photos
- `data/rf-measurements/` — NanoVNA measurement data (S-parameter sweeps)

## Key Specs

**RF Front End: 1090 MHz Filter-LNA (TA1090EC + PGA-103+)**

| Parameter | Value | Source |
|-----------|-------|--------|
| Cascade gain @ 1.09 GHz | ~13.3 dB | Friis analysis (sim + datasheet) |
| System noise figure | ~3.2 dB | Friis cascade |
| LNA gain (S21) @ 1.09 GHz | ~15.6 dB | QucsStudio simulation |
| LNA noise figure | ~0.9 dB | PGA-103+ datasheet |
| Input match (S11) @ 1.09 GHz | −13.7 dB | QucsStudio simulation |
| Front filter | TA1090EC SAW, ~2.3 dB insertion loss | TAI-SAW datasheet |
| Topology | SAW filter → PGA-103+ E-pHEMT LNA, bias-tee | — |
| Port impedance | 50 Ω in/out (no external matching) | — |
| Supply | 4.5 V via bias-tee choke | — |

*Simulated/calculated values pending hardware validation. NanoVNA measurement data in `/data/rf-measurements/`.*

**Receiver**

- Nooelec NESDR SMArt v5 SDR with software-activatable bias tee
- dump1090 decode on Raspberry Pi 5
- Coverage: PDX airspace (~130-140 nm radius)

## Design Notes

The front end places a SAW filter ahead of the LNA (filter-first topology). This trades a small noise-figure penalty, the filter's ~2.3 dB insertion loss adds directly to system NF per the Friis equation, for out-of-band rejection of FM broadcast, cellular, and GSM signals that would otherwise overload the SDR in a high-RF environment near an airport. The PGA-103+ was chosen for its sub-1 dB noise figure and 50 Ω matched ports requiring no external matching network.

## Roadmap

1. Finalize schematic and PCB layout; export Gerbers
2. Fabricate and assemble the filter-LNA board
3. NanoVNA validation against simulated gain / match
4. Integrate measured front-end performance into system characterization
5. Complete anomaly-detection pipeline and dashboard

## References

- [Mode S / ADS-B protocol (mode-s.org)](https://mode-s.org/)
- [dump1090 (flightaware fork)](https://github.com/flightaware/dump1090)
- [RTL-SDR Blog V4](https://www.rtl-sdr.com/rtl-sdr-blog-v4-release/)
- [PGA-103+ datasheet (Mini-Circuits)](https://www.minicircuits.com/pdfs/PGA-103+.pdf)

## License

MIT
