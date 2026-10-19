# Offline-Multilingual-Assistive-Device-for-Visually-Impaired-Individuals.


> Software implementation for an offline, multilingual assistive device for visually impaired individuals. Features Tesseract OCR (95% English, 78.8% Sinhala accuracy) and fine-tuned Sinhala/English Tacotron2 & WaveGlow neural text-to-speech models.

---

## Project Overview

Visual impairment affects millions globally, limiting independence, education, and social participation. While modern assistive technologies have advanced, they frequently depend on external smart devices and continuous internet connectivity, reducing their usability in real-world settings. Furthermore, mainstream tools are heavily biased toward widely spoken languages like English, leaving regional languages without adequate alternatives.

This project develops an offline, portable assistive device designed to convert printed text into natural speech in both **English** and **Sinhala**. By eliminating internet dependency, the system ensures that visually impaired individuals—particularly in Sri Lanka—can access printed educational materials, signage, and documents independently and in real-time.

---

 ## Proposed Methodology

The architecture integrates Optical Character Recognition (OCR) to extract text from physical media with neural Text-to-Speech (TTS) models for high-fidelity audio synthesis.

```text
[ Printed Text/Image ] 
       │
       ▼
[ Tesseract OCR Module ] (Grayscale ➔ Adaptive Thresholding ➔ Resizing)
       │
       ▼
[ Extracted Text ]
       │
       ├─────────────────────────┐
       ▼                         ▼
[ English TTS Pipeline ]   [ Sinhala TTS Pipeline ]
(Pre-trained Tacotron2/     (Transfer Learning: Pre-trained Tacotron2 
 WaveGlow on LJSpeech)       fine-tuned on Google Open Speech Dataset)
       │                         │
       └─────────────────────────┘
                 │
                 ▼
       [ Synthetic Audio Output ] (Designed for Edge/Raspberry Pi Hardware)

```

### 1. Optical Character Recognition (OCR) Integration

The system employs **Tesseract OCR** due to its inherent offline capabilities and native support for customizability across complex scripts like Sinhala.

* **Preprocessing Pipeline:** Before text extraction, images undergo crucial enhancement steps:
* *Grayscale Conversion:* Minimizes background noise and optimizes text clarity.


* *Adaptive Thresholding:* Sharply improves the visual contrast of characters against the background.


* *Resizing:* Scales image dimensions to optimize recognition accuracy for stylized or smaller font sizes.





### 2. Speech Synthesis & Transfer Learning Pipeline

Generating high-quality speech for low-resource languages poses severe data and computational constraints. The system resolves this via deep learning architectures:

* **Core Neural Models:** Uses **Tacotron 2** (a sequence-to-sequence encoder-decoder attention network) to convert input text strings into mel-spectrograms, paired with the **WaveGlow** flow-based vocoder to directly synthesize realistic audio waveforms.


* **English Synthesis:** Leverages NVIDIA's pre-trained Tacotron 2 and WaveGlow models trained on the public-domain **LJSpeech Dataset** (24 hours of single-speaker English speech recordings).


* **Sinhala Synthesis via Transfer Learning:** Initial training from scratch yielded unclear, noisy audio due to data scarcity. To solve this, **transfer learning** was implemented by taking a pre-trained English Tacotron 2 model and fine-tuning it on the **Sinhala TTS Dataset** (comprising ~20 hours of multi-speaker audio from Google’s Open Speech and Language Resources) for 150 epochs. WaveGlow was utilized for inference without extra training, successfully adapting to Sinhala phonetics while minimizing resource overhead.



### 3. Hardware Deployment Strategy

The system architecture is engineered for low-power edge deployment using a **Raspberry Pi** paired with a camera module and audio speakers. Both Tesseract OCR and the compressed neural TTS frameworks are optimized to run fully untethered in outdoor and non-networked environments.

---

## Results and Discussion

### A. Optical Character Recognition (OCR) Performance

* **English Text:** Evaluated across 621 words, yielding 30 misrecognized terms, a low Word Error Rate (WER) of **4.8%**, and an overall recognition accuracy of **95%**. Errors were primarily isolated to suboptimal lighting conditions (shadows or overexposure).


* **Sinhala Text:** Evaluated across 528 words, yielding 112 errors, a WER of **21.2%**, and an accuracy of **78.8%**. While lower than English, this performance is acceptable given the deep script complexities, diacritical marks, and complex letter combinations inherent to Sinhala.



| Language | Total Words | Errors | Accuracy (%) |
| --- | --- | --- | --- |
| **English** | 621 | 30 | **95.0%**<br> |
| **Sinhala** | 528 | 112 | **78.8%**<br> |

### B. Text-to-Speech (TTS) Performance

* **English TTS:** Evaluated on a 582-word mixed content test generating a 4-minute audio output. It achieved a stellar error rate of **~2.6%** (only 15 total errors, consisting of 7 major mispronunciations/stresses and 8 minor pause inconsistencies).


* **Sinhala TTS & Transfer Learning:** Initial baseline training produced distorted audio. However, implementing transfer learning over 150 epochs showed steady loss reduction and the progressive appearance of energy bands in generated mel-spectrograms. While further epochs and larger dataset expansions will enhance production clarity, the results validate the viability of transfer learning for low-resource neural TTS systems.


## References

1. World Health Organization, "Up to 45 million blind people globally — and growing," World Health Organization, 2003. [Online]. Available: [https://www.who.int/news/item/09-10-2003-up-to-45-million-blind-people-globally---and-growing](https://www.who.int/news/item/09-10-2003-up-to-45-million-blind-people-globally---and-growing).


2. S. N. Velmurugan, S. Sekar, B. S. Jebaraj, D. Vishnu Shakthi, K. Azarudeen, and R. B. Shanmuga, "A Next-Gen Automated Reading Companion for the Visually Impaired," in *2023 Int. Conf. on Research Methodologies in Knowledge Management, Artificial Intelligence and Telecommunication Engineering (RMKMATE)*, Chennai, India, 2023, pp. 1–7, doi: 10.1109/RMKMATE59243.2023.10369761.


3. P. Manage, V. Ambe, P. Gokhale, V. Patil, R. M. Kulkarni, and P. R. Kalburgimath, "An Intelligent Text Reader based on Python," in *2020 3rd Int. Conf. on Intelligent Sustainable Systems (ICISS)*, Thoothukudi, India, 2020, pp. 1–5, doi: 10.1109/ICISS49785.2020.9315996.


4. M. Prabha, P. Saraswathi, J. Hailly, C. Sindhuja, and P. Udhaya, "Smart Glasses: A Visual Assistant for Visually Impaired," in *2023 11th Int. Conf. on Emerging Trends in Engineering & Technology - Signal and Information Processing (ICETET - SIP)*, Nagpur, India, 2023, pp. 1–6, doi: 10.1109/ICETET-SIP58143.2023.10151485.


5. C. K. Amarasinghe, R. D. S. P. Pinto, and K. N. Sudusinghe, "Guardian - Smart Assistant Tool for Visually Impaired People," in *2022 4th Int. Conf. on Advancements in Computing (ICAC)*, Colombo, Sri Lanka, 2022, pp. 429–434, doi: 10.1109/ICAC57685.2022.10025262[cite: 5].
6. K. Sharma and P. Syal, "Real Time Object Detection for Assisting Visually Impaired People," in *2022 OPJU Int. Technology Conf. on Emerging Technologies for Sustainable Development (OTCON)*, Raigarh, Chhattisgarh, India, 2022, pp. 1–6, doi: 10.1109/OTCON56053.2023.10114050[cite: 6].
7. T. K. Arachchige and R. Weerasinghe, "TacoSi: A Sinhala text-to-speech system with neural networks," in *2023 3rd Int. Conf. on Advanced Research in Computing (ICARC)*, Belihuloya, Sri Lanka, 2023, pp. 120–124, doi: 10.1109/ICARC57651.2023.10145749.
8. S. K. Konathala, Z. Kondepudi, B. Vommi, Y. Genjii, R. Dogga, and A. Viyyapu, "Assistive glasses for blind people," in *2023 7th Int. Conf. on Computing, Communication, Control and Automation (ICCUBEA)*, Pune, India, 2023, pp. 1–6, doi: 10.1109/ICCUBEA58933.2023.10392252.
9. J. Shen *et al.*, "Natural TTS synthesis by conditioning WaveNet on mel spectrogram predictions," in *IEEE Int. Conf. on Acoustics, Speech, and Signal Processing (ICASSP)*, 2018.
10. A. Debnath, S. S. Patil, G. Nadiger, and R. A. Ganesan, "Low-resource end-to-end Sanskrit TTS using Tacotron2, WaveGlow and transfer learning," in *2020 IEEE 17th India Council Int. Conf. (INDICON)*, New Delhi, India, 2020, pp. 1–5, doi: 10.1109/INDICON49873.2020.9342071.
11. Google, Inc., "Sinhala TTS: Sinhalese multi-speaker TTS corpora," 2015-2016. [Online]. Available: [https://openslr.org/30/](https://openslr.org/30/).
12. K. Ito, "LJSpeech: A public domain speech dataset," 2017. [Online]. Available: [https://keithito.com/LJ-Speech-Dataset/](https://keithito.com/LJ-Speech-Dataset/).
