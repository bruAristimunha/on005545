[![DOI](https://img.shields.io/badge/DOI-10.82901%2Fnemar.on005545-blue)](https://doi.org/10.82901/nemar.on005545)

This dataset, used in the analysis reported by Kanno et al., (2025), contains intracranial EEG recordings from 106 individuals who performed an auditory‑naming task. The corresponding MATLAB analysis code is available at https://github.com/a8k8nn0/TractographyAtlas, and electrode coordinates are provided in MNI‑305 space.

Each EDF file is tagged for the auditory naming task with the following event codes:  
401 – stimulus onset  
402 – stimulus offset  
501 – response onset  

Reference: 
Aya Kanno, Ryuzaburo Kochi, Kazuki Sakakura, Yu Kitazawa, Hiroshi Uda, Riyo Ueda, Masaki Sonoda, Min-Hee Lee, Jeong-Won Jeong, Robert Rothermel, Aimee F. Luat, Eishi Asano. Dynamic Causal Tractography Analysis of Auditory Descriptive Naming: An Intracranial Study of 106 Patients. bioRxiv 2025.03.07.641428; doi: https://doi.org/10.1101/2025.03.07.641428

## Channel names follow the data headers (18 Oct 2026)

In 336 recordings the channel list of channels.tsv did not equal the data-file header (names, order or count). In the 336 with a different channel count, `mne_bids.read_raw_bids` warned and kept the header names, so the channels.tsv types and bad-channel status reached only the channels whose names happen to be identical; a reader that applies the channels.tsv rows by position gives wrong labels wherever the two lists are not aligned. Since 18 Oct 2026 every channels.tsv lists exactly the header channels, in header order, with the header names (MNE names: duplicated header labels numbered -0, -1).

Rows were matched to header channels BY NAME, never by position (exact; then case/whitespace; then the recorder prefix 'EEG '/'POL ', '_' and zero padding; then a trailing '-<reference>' token). Matched rows keep all their columns (type, units, status, ...); 0 rows took the header name.

16864 header channels had no channels.tsv row (336 files) and got a new row: type from the name (ECG/EMG/EOG/trigger) or from the other contacts of the same shaft, else MISC; units from the header; status n/a. Examples: `sub-01_ses-01_task-auditory_run-01_ieeg.edf`: POL A1-A2, POL A9-A10, POL A17-A18, POL A25-A26; `sub-01_ses-02_task-auditory_run-01_ieeg.edf`: POL A40-A48, POL A41-A49, POL A42-A50, POL A43-A51; `sub-01_ses-03_task-auditory_run-01_ieeg.edf`: POL B1-B2, POL B17-B18, POL B25-B26, POL B33-B34.

43524 channels.tsv rows had no channel in the data file (336 files) and were removed; they are kept verbatim under `sourcedata/channels-removed-2026-10-18/` (same relative path). Examples: `sub-01_ses-01_task-auditory_run-01_ieeg.edf`: A1, A2, A3, A4, A5; `sub-01_ses-02_task-auditory_run-01_ieeg.edf`: A1, A2, A3, A4, A5; `sub-01_ses-03_task-auditory_run-01_ieeg.edf`: A1, A2, A3, A4, A5.

Applied by position, the former rows would have given wrong labels to 16864 channels in 336 files (rows missing, extra or out of order before that position).

Bipolar data: in 333 files the data header holds bipolar derivations named after two contacts (e.g. `POL A1-A2`), while channels.tsv listed the monopolar contacts (`A1`, `A2`, ...). A derivation is not matched to the row of its first contact: each of the 16192 bipolar channels got its own row (type = the type of both contacts, `reference` = the second contact, columns shared by both contact rows copied, `status` bad when either contact was bad (5651 rows), good when both were good (10541 rows)), and the contact rows not recorded as such moved to `sourcedata/channels-removed-2026-10-18/`. electrodes.tsv (contact positions) is unchanged; a derivation `X-Y` lies between contacts X and Y.

## Electrode locations (19 Oct 2026)

Add coordsystem.json MNI305/mm next to 336 electrodes.tsv (proposal A1, Lane G round 4). Evidence: Wayne State (Asano/Kanno/Kochi) deposits use FreeSurfer-standard (MNI305/fsaverage) electrode coordinates; MNI305 is a millimetre space by definition. No OpenNeuro S3 version of this dataset ever held a coordsystem.json (all versions listed 2026-10-19). Paper PMC12355976 (section 2.2): "spatially normalized all electrode locations to the FreeSurfer standard brain space"; OpenNeuro ds005545 README: "electrode coordinates are provided in MNI-305 space"; 280 of the 336 electrodes files have x/y/z byte-identical to ds006234 (README MNI-305) and 178 to on007118-20 (deposited coordsystem fsaverage / mm).

Full evidence per dataset: REPORT-ieeg-locations.md and ieeg_locations.json (Lane G, 19 Oct 2026). This dataset mirrors OpenNeuro; the same change belongs upstream. Local branch only.
