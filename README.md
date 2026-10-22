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

## Electrode locations (19 Oct 2026; revised 22 Oct 2026)

Add coordsystem.json MNI305/mm next to 336 electrodes.tsv. Evidence: Wayne State (Asano/Kanno/Kochi) deposits use FreeSurfer-standard (MNI305/fsaverage) electrode coordinates; MNI305 is a millimetre space by definition. No OpenNeuro S3 version of this dataset ever held a coordsystem.json (all versions listed 2026-10-19). Paper PMC12355976 (section 2.2): "spatially normalized all electrode locations to the FreeSurfer standard brain space"; OpenNeuro ds005545 README: "electrode coordinates are provided in MNI-305 space".

Primary source: this dataset's own README (line 3 of README.md on main, nemarDatasets/on005545@948f2825), quoted above.

Reproducible check (22 Oct 2026): for each of the 336 electrodes.tsv, every contact's x/y/z was compared (to 0.001 mm) with every electrodes.tsv of other deposits from the same group at pinned nemarDatasets commits (on004551@c4c035f6, on004859@db21ebb2, on005007@b59363aa, on005931@cbd529e5, on006107@b37dcbfc, on006233@396692f0, on006910@9eb96ebd, on006914@1956b035, on007118@8f2c41fa, on007119@c099745b, on007120@66296ebd). 330 files have all contacts identical to one file of another deposit: 216 to a deposit whose coordsystem.json declares fsaverage/mm (on007118, on007119, on007120) and 310 to a deposit whose README states MNI305/MNI-305 (on004551, on006233, on006910, on006914); a file can match both. The other 6 match partly (3 with at least 89 % of contacts identical, 0 with 50-89 %, 3 below 50 %, 0 none).

Limitation: these sidecars declare the frame and units only. electrodes.tsv is unchanged and no coordinate value changed. Positions do not attach to channels in mne_bids or EEGDash, because the channel names (data-header labels, e.g. prefixed labels or bipolar derivations) differ from the contact names in electrodes.tsv; a channel-to-contact mapping is not part of this change. This dataset mirrors OpenNeuro; the same change belongs upstream.
