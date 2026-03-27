# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.14.1] - 2026-03-27

### Added
- Added `.gitignore` with a whitelist approach to focus on documentation and core configuration.
- Initialized comprehensive version history in `CHANGELOG.md`.

### Changed
- Renamed `00-README.md` to `README.md` in the documentation folder for standard naming.
- Consolidated project structure for public release.

---

## [1.14.0] - 2026-03-26
### Changed
- Final refinements to documentation before archiving version 1.14.

## [1.13.0] - 2026-03-20
### Improved
- Documentation consolidation: Refined `02-technical-deep-dive.md` with detailed implementation notes.
- Performance optimizations for 3D model loading.

## [1.12.0] - 2026-03-10
### Changed
- Internal architecture cleanup and shared script refinements.

## [1.11.0] - 2026-02-28
### Added
- Comprehensive Zoom Compensation System (`zoomCompensation.js`) to handle HiDPI and system scaling.
- Mobile and tablet detection for adaptive layout scaling.

## [1.10.0] - 2026-02-15
### Added
- `CV-RESUME-DATA.md` with structured career data.
- Expanded technical documentation regarding Firebase initialization.
### Fixed
- Duplicate Firebase app instances bug resolved via centralized `firebase-init.js`.

## [1.9.0] - 2026-02-01
### Added
- Dual-sticky Projects Banner with smooth horizontal marquee.
- Low-spec mode exemptions for essential UI animations.

## [1.8.0] - 2026-01-20
### Added
- Guest Authentication System for secure NDA project viewing.
- Session management with localStorage persistence and expiration.
- Floating NDA Reminder Box for authenticated sessions.

## [1.5.0] - 2025-12-28
### Added
- NDA Access System with manual approval workflow.
- Automated PDF generation for signed NDA agreements using jsPDF.
- Email notifications for admin via EmailJS.
- Cookie Consent System (GDPR-compliant) with analytics control.
- Private project visibility settings in Admin Dashboard.

## [1.1.0] - 2025-11-15
### Added
- Initial production-ready release.
- Three.js 3D project showcase integration.
- Multi-theme system (Day, Night, Cyberpunk, Monochrome, Blueprint).
- Full Admin Dashboard (Projects, Blog, About Me, Resume, Milestones).
- Firebase Authentication and Firestore integration.
- BIOS-style landing page.
