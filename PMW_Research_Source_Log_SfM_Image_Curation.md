# PMW Research Source Log
**Charbagh Collective — Platform & Web Engineering Track**
**Author:** Humna Attique (Roll No. 24I-3097, SE-C)
**Supervisor:** Waleed Ajmal

---

## 1. Research Question

**Why did tight image curation raise the Shalimar Gardens COLMAP reconstruction from 2 registered cameras / 42 sparse points (V1) to 15/15 cameras / 842 points (V2), while the Derawar Fort pipeline stayed limited — and what image-capture and overlap practices explain that gap?**

This is narrow enough to answer with evidence (it's a direct before/after comparison from my own pipelines) but general enough to produce a reusable method note for any teammate running COLMAP on a new heritage site.

---

## 2. Annotated Sources

### Source 1
- **Title:** COLMAP Tutorial — Structure-from-Motion Pipeline (official docs)
- **Link:** https://colmap.readthedocs.io/en/latest/tutorial.html
- **Date:** COLMAP 3.6 documentation (maintained, no single publish date; actively current)
- **Credibility:** Primary source — written by the tool's own developers (Schönberger & Frahm, ETH Zurich / UNC Chapel Hill). This is the authoritative reference for how COLMAP's SfM stages actually work.
- **Summary (my own words):** The docs confirm that SfM needs a set of *overlapping* images of the same object from different viewpoints to recover both the 3D points and each camera's position. It explicitly recommends capturing images with good texture and controlled overlap if you have any control over the shoot. This is the direct technical explanation for why my broadly-sourced V1 image set (varied angles, inconsistent overlap) only registered 2 cameras, while the tightly-curated Moorcroft Pavilion set in V2 gave COLMAP enough shared features to register all 15.

### Source 2
- **Title:** COLMAP — Structure-from-Motion and Multi-View Stereo (project homepage, ETH Zurich / Schönberger)
- **Link:** https://colmap.github.io/
- **Date:** Actively maintained; original papers cited are Schönberger & Frahm, CVPR 2016
- **Credibility:** Primary source, peer-reviewed foundation (CVPR is a top-tier computer vision venue). Cited as the canonical reference across nearly all photogrammetry literature I found for this log.
- **Summary:** Establishes COLMAP as an incremental SfM system — it grows the reconstruction image-by-image as it finds enough matched features to solve for a new camera's pose. This matters for my comparison: incremental registration means one poorly-connected image (weak overlap) can stall the whole chain, which is consistent with why Derawar Fort's more scattered image set produced weaker registration than Shalimar's curated set.

### Source 3
- **Title:** Photogrammetric Applications for Cultural Heritage — Guidance for Good Practice (Historic England, Jon Bedford)
- **Link:** https://hf-files-oregon.s3.amazonaws.com/hdpkorecgroup_kb_attachments/2018/05-23/1a65319e-33e5-4f9a-962e-dd0a053502c8/1.1-Photogrammetric-Applications-for-Cultural%20Heritage.pdf
- **Date:** Published October 2017, Historic England
- **Credibility:** High — Historic England is the UK government's official heritage body; this guidance is widely cited as a field standard for heritage photogrammetry (also referenced by later academic literature I found, e.g. ICCM Foundation).
- **Summary:** Walks through the full SfM/MVS workflow — overlapping photographs, tie-point detection across images, then calculating camera orientation and position from those tie points. It stresses that planning the *capture strategy* (how much overlap, how consistent the framing) directly determines processing quality, not just the software settings. This backs up my practical finding: the difference between V1 and V2 wasn't a COLMAP parameter change, it was image discipline going in.

### Source 4
- **Title:** Photogrammetry for Archaeological Documentation and Cultural Heritage Conservation (Al-Ruzouq, 2012)
- **Link:** https://www.researchgate.net/publication/224831011_Photogrammetry_for_Archaeological_Documentation_and_Cultural_Heritage_Conservation
- **Date:** April 2012
- **Credibility:** Peer-reviewed academic publication (ResearchGate-hosted); older but still commonly cited baseline in heritage-photogrammetry literature.
- **Summary:** Frames photogrammetry's core requirement as pairs (or larger sets) of *overlapping* photos used to build 3D geometry, replacing older hand-drawn or plan-based documentation. This gave me the historical/methodological context for why overlap-first capture, not post-processing, is treated as the foundational constraint across the field — reinforcing that Derawar Fort's limitation was upstream in capture, not something a different COLMAP flag could fix.

### Source 5
- **Title:** Capturing the Past, Shaping the Future: A Scoping Review of Photogrammetry in Cultural Building Heritage (MDPI, 2025)
- **Link:** https://www.mdpi.com/2079-9292/14/18/3666
- **Date:** Published September 2025 (recent)
- **Credibility:** Peer-reviewed, published in MDPI Electronics; scoping review format, so it synthesizes many prior studies rather than presenting one dataset — useful for a current, broad view of where the field stands in 2025/2026.
- **Summary:** Describes how modern heritage documentation pairs photogrammetry with systematic image acquisition strategies (including UAV capture) specifically to guarantee sufficient, consistent overlap across hard-to-reach or complex structures. It reinforced why fort-type sites (like Derawar) are harder than a single garden pavilion (like Shalimar's Moorcroft Pavilion): larger, more irregular structures need a deliberately planned capture pattern, not an ad hoc photo set, to hit the overlap SfM needs.

### Source 6 (supporting)
- **Title:** Multi-image Photogrammetry as a Practical Tool for Cultural Heritage Survey and Community Engagement (ScienceDirect)
- **Link:** https://www.sciencedirect.com/science/article/abs/pii/S0305440314000132
- **Date:** January 2014
- **Credibility:** Peer-reviewed, published in a heritage-science journal; widely cross-referenced by other sources in this log.
- **Summary:** Defines "multi-image photogrammetry" as the modern, batch-processed evolution of stereo-pair photogrammetry, where large sets of overlapping images are fed into automatic camera calibration and reconstruction software (i.e., what COLMAP does). It gave me the vocabulary to describe my own V1→V2 shift accurately: I moved from a small, loosely-related image set toward the batch, high-overlap capture this literature treats as the field standard.

---

## 3. Connection to PMW Deliverable

This source log directly supports and explains an existing Charbagh Collective deliverable: the **Shalimar Gardens COLMAP SfM pipeline** (V1 → V2 iteration) and the parallel **Derawar Fort pipeline**, both under `humnaattique4-sys`. Instead of just reporting "V2 was better," this research grounds *why* in documented SfM/photogrammetry practice, and produces a reusable method note the rest of Charbagh Collective can apply before shooting images for any future site (e.g. Lahore Fort follow-ups).

---

## 4. Research-to-Deliverable: Technical Method Note

*(Drop-in section for the team presentation page, dev.to write-up, or deck notes)*

> **Why image curation, not COLMAP settings, decided our reconstruction quality**
>
> Our first Shalimar Gardens attempt used a broad, loosely-related set of images and registered only 2 of the intended cameras, producing a sparse 42-point cloud. Official COLMAP guidance is explicit that its incremental SfM pipeline depends on images sharing strong, consistent overlap — without it, the pipeline can't solve for a new camera's pose, and the chain stalls early. Heritage-photogrammetry field guidance (Historic England's *Good Practice* handbook, and subsequent academic literature) treats capture planning, not software configuration, as the primary lever for reconstruction quality.
>
> When we re-shot and curated a tightly-overlapping image set focused on the Moorcroft Pavilion, the same pipeline registered all 15 cameras and produced 842 sparse points — a ~20x gain with no change to COLMAP's parameters. Our parallel Derawar Fort attempt, constrained by a larger, more irregular structure and less consistent image overlap, stayed limited for the same reason the literature predicts: forts and large complex sites need deliberately planned, high-overlap capture patterns (recent UAV-based heritage documentation reviews confirm this is now standard practice for exactly this kind of site), not just a bigger photo set.
>
> **Takeaway for future Charbagh Collective shoots:** plan the shot list around guaranteed overlap between adjacent images *before* shooting, especially for large or irregular heritage structures — this determines reconstruction quality more than any COLMAP flag does.

---

## 5. Evidence to Attach Before Submitting

- [ ] Link to Shalimar Gardens repo (V1 and V2 commits/tags showing the `.ply` outputs)
- [ ] Link to Derawar Fort repo
- [ ] Screenshot comparison: V1 sparse cloud (2 cameras/42 pts) vs V2 (15/15 cameras/842 pts)
- [ ] Screenshot of this source log or the dev.to section where it's published
- [ ] Optional: prompt log if AI tools were used to help synthesize sources

---

## 6. Time Claim

**Work window:** *(fill in your actual start–end time/date)*
**Hours claimed:** *(claim only the real time spent researching, annotating, and writing the method note — up to the 4-hour sprint maximum)*
