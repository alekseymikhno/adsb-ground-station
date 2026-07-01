# ADS-B 1090 MHz Receiver — PDX Airspace

A complete 1090 MHz ADS-B receiver system tracking aircraft over Portland, OR, with an in-progress custom RF front end and LLM-based anomaly detection.

## Status

- **Pi 5 receiver**: Live and collecting flight data (May–July 2026)
- **Custom RF front end**: Fully designed (Friis cascade: ~2.85 dB NF, +32 dB gain target), pending PCB fabrication
- **Anomaly detection**: LLM analysis pipeline in development

## System Architecture

[Insert architecture diagram here — or reference docs/system-architecture.png]

## What It Does

1. **RF capture**: 1090 MHz antenna → filter-LNA-filter chain → RTL-SDR Blog V4 I/Q sampler
2. **Decode**: Raspberry Pi 5 runs dump1090 to decode Mode S/ADS-B messages in real time
3. **Analysis**: LLM-based anomaly detector identifies unusual flight patterns over PDX airspace
4. **Output**: Logged to SQLite, served via web dashboard

## Folder Structure

- `hardware/rf-frontend/` — KiCad schematic, PCB, simulation files, BOM
- `software/receiver/` — dump1090 config, startup scripts
- `software/anomaly-detection/` — Analysis pipeline code
- `docs/` — Friis cascade analysis, system diagrams, reference photos
- `data/sample-logs/` — Sample decoded messages and flight tracks

## Key Specs

- **Simulated NF** (Friis cascade): ~2.85 dB typical, 3.85 dB worst-case
- **Chain gain**: +31.9 dB
- **Front-end architecture**: TA1090EC SAW filter → PMA4-33GLN+ MMIC → TA1090EC SAW filter
- **Receiver**: RTL-SDR Blog V4 with software-activatable bias tee
- **Decode**: dump1090 on Raspberry Pi 5
- **Coverage**: PDX airspace (approximately 80 nm radius)

## Roadmap

1. Fabricate and assemble RF front-end PCB
2. NanoVNA validation against Friis prediction
3. Integrate measured front-end performance into system characterization
4. Complete anomaly-detection pipeline and dashboard
5. Publish full data set and analysis results

## References

- [Mode S / ADS-B protocol](http://www.Mode-S.org/)
- [dump1090 documentation](https://github.com/antirez/dump1090)
- [RTL-SDR Blog V4](https://www.rtl-sdr.com/rtl-sdr-blog-v4-release/)

## License

MIT
