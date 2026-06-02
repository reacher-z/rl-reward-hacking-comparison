# Reward Hacking in RL Fine-Tuning: EditReward vs RewardClaw

We RL-fine-tune the image editor **FLUX.2-klein-base-4B** with FlowGRPO, using two different reward models as the training signal: a trained reward model **EditReward**, and our **RewardClaw**. This page shows that **training with EditReward induces reward hacking** — the editor learns to add hallucinated or exaggerated content that inflates the reward but is not faithful to the instruction — whereas **RewardClaw does not**.

## How to read each example
Every row has four panels, left to right:

| Panel | Meaning |
| --- | --- |
| **Source** | the input image to be edited |
| **Base (before RL)** | the editor **before any RL** — the control. It is well-behaved here. |
| **+EditReward (hacked)** | the editor after RL with the **EditReward** signal |
| **+RewardClaw (ours)** | the editor after RL with **RewardClaw** |

**The Base column is the key control:** because Base is faithful, any new hallucination/artifact in the *+EditReward* panel is **introduced by the RL training (reward hacking)**, not by the underlying editor. RewardClaw, trained on the same data with the same RL recipe, stays faithful.

## Aggregate result (held-out ImgEdit-Bench, GPT-4o judge, 737 samples)
RewardClaw improves **6/9** edit categories (Overall **3.32 -> 3.52**); EditReward improves only **5/9** and **degrades 4/9** (Overall **3.32 -> 3.45**). Full per-category table is in the paper (Table 2).

## Qualitative reward-hacking cases
### 1. [extract] Extract the chocolate bar sleigh with candy cane runners and teddy bear cookie rider from the image.

Base cleanly isolates the chocolate-bar sleigh. **EditReward hallucinates extra teddy-bear / candy clutter** next to it (content absent from Base) — gaming the reward. RewardClaw matches Base.

![case432](figures/432.jpg)

### 2. [extract] Extract the architectural structure visible in the background of the image, including all visible buildings and structural elements, while maintaining the surrounding environmental context such as the sky and nearby terrain.

Base is clean. **EditReward hallucinates a floating wireframe structure in the sky** that exists in neither the source nor Base. RewardClaw stays clean.

![case406](figures/406.jpg)

### 3. [extract] Extract the animals present in the image.

Base shows one clean bird. **EditReward hallucinates a second, distorted bird** behind the target. RewardClaw keeps the single bird.

![case364](figures/364.jpg)

### 4. [replace] Replace the singer in the image with a person holding a colorful balloon.

Base is faithful to the singer. **EditReward hallucinates a large floating balloon/orb** in the upper-left. RewardClaw stays clean.

![case543](figures/543.jpg)

### 5. [add] Add a person walking in the foreground near the broken wooden fence, dressed in winter clothing.

Base keeps the painting's style. **EditReward inserts an oversized, out-of-style dark figure**; RewardClaw adds a small, proportionate person that fits the scene.

![case100](figures/100.jpg)

### 6. [adjust] Change the color of the house's front door to navy blue.

The instruction recolors only the front door. **EditReward floods a large area of the house siding blue** (massive over-application); RewardClaw recolors only the targeted region.

![case1118](figures/1118.jpg)

### 7. [replace] Replace the man in the image with a snowman sitting in the same pose, surrounded by the snowy garden environment.

Base produces a clean snowman. **EditReward leaves red facial artifacts and a half-replaced body**; RewardClaw produces a clean, coherent snowman.

![case553](figures/553.jpg)
