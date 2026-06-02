# Reward Hacking in RL Fine-Tuning: EditReward vs RewardClaw

We RL-fine-tune the image editor **FLUX.2-klein-base-4B** with FlowGRPO, using two reward models as the training signal: **EditReward** (a trained reward model) vs our **RewardClaw**. Training with EditReward induces **reward hacking** — the editor learns to add hallucinated/exaggerated content or revert to the source to inflate the reward, instead of faithfully following the instruction. RewardClaw, trained on the same data with the same recipe, does not.

Each row: **Source | Base (before RL) | +EditReward (hacked) | +RewardClaw (ours)**.

**Base (before RL) is the control:** it is faithful, so any new artifact or reverted content in the *+EditReward* panel is **introduced by the RL training (reward hacking)**.

Aggregate (held-out ImgEdit-Bench, GPT-4o judge, 737 samples): RewardClaw improves **6/9** edit categories (Overall **3.32 -> 3.52**); EditReward improves 5/9 and degrades 4/9 (Overall **3.32 -> 3.45**).

## Reward-hacking cases

### 1. [add] Add a dog walking beside the person on the snow-covered path.

*EditReward adds extra large foreground dogs not beside person; RewardClaw keeps dog near person with only mild restyle*

![case103](figures/103.jpg)

### 2. [background] Change the trees in the picture to a row of palm trees and adjust the ground to resemble a sandy beach environment.

*EditReward loses sandy beach and reverts muddy ground while RewardClaw preserves palms and beach*

![case240](figures/240.jpg)

### 3. [background] Change the racetrack in the picture from an asphalt circuit to a desert track.

*EditReward reintroduced asphalt circuit/curbs versus BASE; RewardClaw stays faithful to desert track.*

![case252](figures/252.jpg)

### 4. [replace] Replace the man in the image with a snowman sitting in the same pose, surrounded by the snowy garden environment.

*EditReward adds duplicated snowman/body artifacts and leaves man clothing, while RewardClaw keeps a single posed snowman in the snowy garden.*

![case553](figures/553.jpg)

### 5. [background] Change the beach in the picture to a mountain landscape.

*EditReward reintroduces palm/beach-like extra content absent from BASE; RewardClaw keeps mountain background faithful.*

![case299](figures/299.jpg)

### 6. [background] Change the wooden surface background in the picture to a marble countertop.

*EditReward kept/reintroduced wooden plank background while RewardClaw stayed faithful with marble countertop.*

![case292](figures/292.jpg)

### 7. [background] Change the floral surface in the background to a simple white fabric.

*EditReward reintroduced gray branch/floral background; RewardClaw stayed plain white and faithful.*

![case300](figures/300.jpg)

### 8. [background] Replace the lush, green landscape with a desert setting, such as sand dunes and sparse vegetation, while keeping the bicycle in the foreground.

*EditReward reintroduces green landscape and weak desert conversion vs BASE; RewardClaw keeps clean desert scene and bicycle foreground*

![case260](figures/260.jpg)

### 9. [adjust] Change the color of footprint in the sand to red.

*EditReward adds a hallucinated red blotch above the footprint, while RewardClaw keeps the edit localized and faithful.*

![case1127](figures/1127.jpg)

### 10. [style] Transfer the image into a stained-glass cathedral-window style.

*EditReward adds saint/window figures and extra objects beyond BASE; RewardClaw mostly preserves castle/rocks with limited style changes.*

![case679](figures/679.jpg)

### 11. [add] Add a small wooden cabin with a thatched roof near the shoreline on the right side of the lake, partially surrounded by trees to blend naturally into the environment.

*EditReward adds extra cabins/structures and artifacts vs BASE; RewardClaw keeps a single shoreline cabin mostly faithful.*

![case68](figures/68.jpg)
