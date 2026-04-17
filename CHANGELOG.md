# Changelog

All notable changes to this project will be documented in this file.

## [0.0.7]

### Changed
- MoCA Sum (`moca_raw`) calculated field now includes education adjustment: `+ if([moca_edu] < 13, 1, 0)` for +1 point if ≤12 years of education
- MoCA Sum field note updated to `"0-30; +1 point if =< 12 years of education"`
- Documentation: Improved Step 3 (API key), Step 4 (Python setup), Step 5 (API key storage, .gitignore, VS Code setup), and Step 6 (survey mode navigation paths)

## [0.0.6]

### Changed
- Documentation: Added a note about missing API token request options due to insufficient REDCap user permissions (contact REDCap administrator) (#10)
- Documentation: Clarified that Python setup verification output (instrument field counts / pandas version) can vary by environment and package edits (#10)

### Fixed
- MoCA `moca_total_score` is now a `calc` field with the education-adjusted formula instead of a plain text field (#11)
- TAP-M Divided Attention, Executive Control, and Distractibility field name suffixes corrected to match TAP-M CSV export order (#12)

## [0.0.5] - 2025-11-27

### Changed
- Removed question numbering from FAL instruments in both `instruments.py` and `rtg.py` for cleaner field labels when using only subset of questions

## [0.0.4] - 2025-11-27

### Added
- Added comprehensive documentation for all RTG instruments in `docs/instruments.md`
- Added RTG instruments to test suite, expanding from 18 to 75 tests
- Updated `tutorial_existing_instruments.py` to loop over all RTG instruments dynamically

## [0.0.3] - 2025-11-27

### Added
- Added RTG module (`rtg.py`) with 20  instruments:
  - `study_participant_information` - Basic participant demographics
  - `fal_initial_questionnaire` - FAL Initial Questionnaire (English version)
  - `becks_depression_inventory_ii` - Beck's Depression Inventory II
  - `stroke_telephone_interview` - Stroke Telephone Interview
  - `frageboden_grk_p10` - Fragebogen GRK P10
  - `sf12` - SF-12 Health Survey
  - `jebsen_taylor_hand_function_test` - Jebsen-Taylor Hand Function Test
  - `functional_gait_assessment` - Functional Gait Assessment
  - `tapm_divided_attention_dual_task` - TAPM Divided Attention Dual Task
  - `tapm_executive_control` - TAPM Executive Control
  - `tapm_gonogo` - TAPM Go/No-Go Test
  - `tapm_distractibility` - TAPM Distractibility
  - `moca` - Montreal Cognitive Assessment (RTG version)
  - `aachener_aphasia_test` - Aachener Aphasia Test
  - `modified_ashworth_scale` - Modified Ashworth Scale
  - `upper_extremity_fuglmeyer_assessment` - Upper Extremity Fugl-Meyer Assessment
  - `action_research_arm_test` - Action Research Arm Test
  - `modified_frenchay_scale` - Modified Frenchay Scale
  - `stroke_impact_scale` - Stroke Impact Scale
  - `nasatlx` - NASA Task Load Index

## [0.0.2] - 2025-08-19

- Added MoCA instrument to the rcol package.
- Updated tutorial to include MoCA instrument.

## [0.0.1] - 2025-08-18

- Initial release of the rcol package.
- Added core project files: `.gitignore`, `README.md`, and `pyproject.toml`.
- Implemented REDCap instrument template functionality and tutorial.
- Added initial files for the rcol package and removed deprecated template module.

## [vX.Y.Z] - YYYY-MM-DD


Please update this file with each new release, listing features, bug fixes, and other changes.
