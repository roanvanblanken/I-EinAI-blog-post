---
title: "Interpretability & Explainability in AI - 2024"
author: "Roan van Blanken (12999350)"
date: ""
geometry: "a4paper"
output: html_document
---

# Blog Post: Self-Monitored Inference-Time Intervention for Generative Music Transformers (SMITIN)

### Roan van Blanken (12999350)
#### Teaching Assistants: Marcel Vélez Vásquez MSc & C.M. Pouw MSc

# Introduction

Recent advancements in generative models have significantly impacted how we create and manipulate audio, especially in music. Musicians and audio professionals constantly seek precise control over their creations, often finding traditional generative models lacking in fine-grained adjustability. To address this gap, researchers have introduced SMITIN (Self-Monitored Inference-Time INtervention), a cutting-edge approach designed to enhance the capabilities of autoregressive generative music transformers [[1](#1), [2](#2), [3](#3), [4](#4), [5](#5), [6](#6), [7](#7)]. This blog post explores how SMITIN [[8](#8)] operates and its potential impact on music generation.

# What is SMITIN?

SMITIN, or Self-Monitored Inference-Time INtervention, is a approach that uses classifier probes to control the output of autoregressive generative music transformers. These probes, which are logistic regression models, are trained on the output of each attention head in the transformer [[9](#9)]. By steering the attention heads based on the probes' indications, SMITIN ensures that the generated music exhibits desired traits, such as the presence of specific instruments or the distinction between real and synthetic music.

# How Does SMITIN Work?

SMITIN integrates classifier probes into each attention head within a pre-trained music transformer model. These probes are trained to detect specific musical characteristics using a small dataset of audio examples. For instance, a probe might be trained to recognize the presence or absence of drums. During music generation, SMITIN dynamically adjusts the intervention strength based on the probes' feedback, ensuring the music generation aligns with the desired characteristics while maintaining coherence [[2](#2)].

## Autoregressive Transformer Models

The foundation of SMITIN lies in autoregressive transformer models like MusicGen. These models generate sequences of audio frames by predicting the next frame based on previous ones. Each transformer layer consists of multiple self-attention heads that process the input to produce the final output [[10](#10)].

## Inference-Time Intervention (ITI)

ITI is a technique where the output of the self-attention heads is modified at inference time to guide the generation process. This modification involves adding a term to the heads based on the classifier probe's direction. The adjustment is controlled to avoid excessive intervention, which could disrupt the temporal coherence of the music [[9](#9)].

Figure 1 illustrates the architecture of SMITIN, showing how the classifier probes are integrated into the attention
heads of the transformer model and how they influence the music generation process.

<div style="text-align: center;">
    <img src="figures/SMITIN_architecture.png" alt="Figure 1 Description" style="width: 97.5%;">
    <p>Figure 1: The overall pipeline of SMITIN for inference-time intervention on a pre-trained music generative transformer. This process enforces specific musical elements (e.g., presence of a particular instrument) during music generation. SMITIN uses a self-monitoring technique to dynamically adjust the intervention strength at each step, allowing precise control over the inclusion of the target characteristic while maintaining the musical quality of the output. </p>
</div>

# Key Features of SMITIN

SMITIN incorporates several advanced features to enhance its performance in music generation tasks. These features include self-monitoring, sparse intervention, and soft-weighting, which collectively ensure the generation of high-quality music while meeting specific desired traits.

## Self-Monitoring

A critical innovation in SMITIN is its self-monitoring capability. The system continuously evaluates the generated
music using the classifier probes. If the probes indicate that the desired musical trait is present, SMITIN reduces
the intervention strength. This self-regulation helps maintain musical quality and prevents overfitting to the intervention.

## Sparse Intervention

To ensure the generated music remains coherent, SMITIN does not intervene at every time step. Instead, it applies interventions at regular intervals, allowing the music to develop naturally while still guiding it toward the desired traits.

## Soft-Weighting

SMITIN introduces a soft-weighting mechanism to balance the influence of different attention heads. Instead of selecting a fixed number of top-performing heads, it assigns weights based on the classifier probes' accuracy. This ensures that the most relevant heads have a greater influence, improving the intervention's effectiveness [[9](#9)].

# Applications and Results

SMITIN has been tested in both audio continuation and text-to-music generation tasks. The results demonstrate significant improvements in achieving desired musical traits without compromising the quality of the generated music.

## Audio Continuation

In audio continuation tasks, SMITIN successfully adds specific instruments to existing music tracks. For example, starting with a track without drums, SMITIN can generate a continuation that seamlessly introduces drums. Objective evaluations show that SMITIN achieves higher success rates in adding instruments compared to baseline methods [[11](#11), [12](#12)].

Figure 2 illustrates the effectiveness of SMITIN in an audio continuation task. Figure 2a shows the waveform of
the original track without drums, while Figure 2b shows the same track with drums added by SMITIN. The results
highlight SMITIN’s ability to integrate new instruments smoothly and naturally into existing music, maintaining the
overall quality and coherence of the track.

<div style="display: flex; justify-content: space-between;">

  <div style="text-align: center;">
    <audio controls>
    <source src="figures/audio_cont_unconditioned.wav" type="audio/mpeg">
    Your browser does not support the audio element.
    </audio>
    <img src="figures/audio_cont_unconditioned.png" alt="Figure 1 Description" style="width: 97.5%;">
    <p>Figure 2a: Drum monitor for original track without drums</p>
  </div>

  <div style="text-align: center;">
    <audio controls>
    <source src="figures/audio_cont_SMITIN_add_drums.wav" type="audio/mpeg">
    Your browser does not support the audio element.
    </audio>
    <img src="figures/audio_cont_SMITIN_add_drums.png" alt="Figure 2 Description" style="width: 97.5%;">
    <p>Figure 2b: Drum monitor for track with drums added by SMITIN</p>
  </div>

</div>

<p style="text-align: center;">Figure 2: Effectiveness of SMITIN in audio continuation tasks</p>


## Text-to-Music Generation

For text-to-music tasks, where music is generated based on textual descriptions, SMITIN enhances the ability to control the presence of specific instruments. By combining text prompts with inference-time interventions, users can achieve more precise and nuanced control over the generated music [[13](#13)].

Figure 3 illustrates the effectiveness of SMITIN in a text-to-music generation task. In this example, the task is to add drums to a track described by the text prompt: ”guitarist talks, tuning notes, tuning pegs, guitar master class, guitar solo, live concert, live audience, soloist, random notes, laughing, bad audio quality, engaging crowds, performer, famous guitarist, tuning legs, fretboard sounds, no fixed tempo, bad audio quality, ambient crowd noises.” Figure 3a shows the drum monitor for the unconditioned track generated from the text prompt, while Figure 3b shows the drum monitor for the track with drums added by SMITIN. The results demonstrate SMITIN’s capability to accurately interpret and apply the text prompt to enhance the musical output with the specified instrument.

<div style="display: flex; justify-content: space-between;">

  <div style="text-align: center;">
    <audio controls>
    <source src="figures/text_to_music_unconditioned.wav" type="audio/mpeg">
    Your browser does not support the audio element.
    </audio>
    <img src="figures/text_to_music_unconditioned.png" alt="Figure 3a Description" style="width: 97.5%;">
    <p>Figure 3a: Drum monitor for unconditioned track from text prompt</p>
  </div>

  <div style="text-align: center;">
    <audio controls>
    <source src="figures/text_to_music_SMITIN_add_drums.wav" type="audio/mpeg">
    Your browser does not support the audio element.
    </audio>
    <img src="figures/text_to_music_SMITIN_add_drums.png" alt="Figure 3b Description" style="width: 97.5%;">
    <p>Figure 3b: Drum monitor for track with drums added by SMITIN</p>
  </div>

</div>


<p style="text-align: center;">Figure 3: Effectiveness of SMITIN in text-to-music generation tasks</p>

# Conclusion

SMITIN represents a significant advancement in the field of generative music models. By incorporating self-monitored inference-time interventions, it provides musicians and audio professionals with the tools to create more controlled and high-quality music. This approach opens up new possibilities for creative expression, allowing for dynamic and precise manipulation of musical elements.

For more details and audio samples demonstrating SMITIN's capabilities, you can visit the [demo page](https://wide-wood-512.notion.site/SMITIN-Self-Monitored-Inference-Time-INtervention-for-Generative-Music-Transformers-Demo-Page-983723e6e9ac4f008298f3c427a23241).

# References

1. Prafulla Dhariwal, Heewoo Jun, Christine Payne, Jong Wook Kim, Alec Radford, and Ilya Sutskever. Jukebox: A generative model for music. arXiv preprint arXiv:2005.00341, 2020.
2. Jade Copet, Felix Kreuk, Itai Gat, Tal Remez, David Kant, Gabriel Synnaeve, Yossi Adi, and Alexandre Défossez. Simple and controllable music generation. arXiv preprint arXiv:2306.05284, 2023.
3. Haohe Liu, Qiao Tian, Yi Yuan, Xubo Liu, Xinhao Mei, Qiuqiang Kong, Yuping Wang, Wenwu Wang, Yuxuan Wang, and Mark D Plumbley. AudioLDM 2: Learning holistic audio generation with self-supervised pretraining. arXiv preprint arXiv:2308.05734, 2023.
4. Shih-Lun Wu, Chris Donahue, Shinji Watanabe, and Nicholas J Bryan. Music controlnet: Multiple time-varying controls for music generation. arXiv preprint arXiv:2311.07069, 2023.
5. Hugo F Flores Garcia, Prem Seetharaman, Rithesh Kumar, and Bryan Pardo. VampNet: Music generation via masked acoustic token modeling. 2023.
6. Felix Kreuk, Gabriel Synnaeve, Adam Polyak, Uriel Singer, Alexandre Défossez, Jade Copet, Devi Parikh, Yaniv Taigman, and Yossi Adi. Audiogen: Textually guided audio generation. 2022.
7. Zalán Borsos, Raphaël Marinier, Damien Vincent, Eugene Kharitonov, Olivier Pietquin, Matt Sharifi, Dominik Roblek, Olivier Teboul, David Grangier, Marco Tagliasacchi, et al. Audiolm: a language modeling approach to audio generation. 2023.
8. Junghyun Koo, Gordon Wichern, Francois G. Germain, Sameer Khurana, and Jonathan Le Roux. Smitin: Self-monitored inference-time intervention for generative music transformers, 2024.
9. Kenneth Li, Oam Patel, Fernanda Viégas, Hanspeter Pfister, and Martin Wattenberg. Inference-time intervention: Eliciting truthful answers from a language model. arXiv preprint arXiv:2306.03341, 2023.
10. Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones, Aidan N Gomez, Lukasz Kaiser, and Illia Polosukhin. Attention is all you need. volume 30, 2017.
11. Zafar Rafii, Antoine Liutkus, Fabian-Robert Stöter, Stylianos Ioannis Mimilakis, and Rachel Bittner. MUSDB18-HQ - an uncompressed version of MUSDB18, August 2019.
12. Igor Pereira, Felipe Araújo, Filip Korzeniowski, and Richard Vogl. MoisesDB: A dataset for source separation beyond 4-stems. arXiv preprint arXiv:2307.15913, 2023.
13. Andrea Agostinelli, Timo I Denk, Zalán Borsos, Jesse Engel, Mauro Verzetti, Antoine Caillon, Qingqing Huang, Aren Jansen, Adam Roberts, Marco Tagliasacchi, et al. Musiclm: Generating music from text. arXiv preprint arXiv:2301.11325, October 2023.
