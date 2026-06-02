# Qualitative Comparison: Reward Hacking in RL Fine-Tuning

FlowGRPO RL fine-tuning of **FLUX.2-klein-base-4B** on ImgEdit-Bench, comparing two reward models used to RL-train the editor: **EditReward** (a trained reward model) vs **RewardClaw** (ours).

On the held-out ImgEdit-Bench, **RewardClaw** improves **6/9** edit categories (Overall 3.32 -> 3.52), while **EditReward** improves only 5/9 and degrades 4/9 (Overall 3.32 -> 3.45). Below, the EditReward-trained editor exhibits **reward hacking** (hallucinated content / global over-restyle to game the reward), while the RewardClaw-trained editor stays faithful.

Each panel: **Source  |  +RL (EditReward)  |  +RL (RewardClaw, ours)**.

## Reward hacking: hallucinated / spurious content from EditReward

**1. [background]** Change the traditional embroidered dress in the picture from a wedding setting to a casual garden setting.

*EditReward hallucinates an entire extra person and restyles the scene; RewardClaw keeps the subject and changes only the background.*

![case288](figures/288.jpg)

**2. [style]** Transfer the image into a hand-sculpted claymation style.

*EditReward inserts hallucinated human figures and barely applies the clay style; RewardClaw cleanly claymates the whole scene.*

![case683](figures/683.jpg)

**3. [style]** Transfer the image into a Lego-brick stop-motion diorama style.

*EditReward scatters random Lego figures and clutter without converting the scene; RewardClaw rebuilds the castle in Lego bricks.*

![case682](figures/682.jpg)

**4. [extract]** Extract the chocolate bar sleigh with candy cane runners and teddy bear cookie rider from the image.

*EditReward adds hallucinated decorations/clutter instead of a clean extraction; RewardClaw isolates the target cleanly.*

![case432](figures/432.jpg)

**5. [extract]** Extract the architectural structure visible in the background of the image, including all visible buildings and structural elements, while maintaining the surrounding environmental context such as the sky and nearby terrain.

*EditReward hallucinates a floating wireframe artifact in the sky; RewardClaw keeps the scene clean.*

![case406](figures/406.jpg)

**6. [add]** Add a set of colorful beach towels hanging over the railing on the right side of the pier.

*EditReward floats the towels in the water off the pier; RewardClaw places them correctly on the right railing.*

![case35](figures/35.jpg)

## Instruction following

**[remove]** Remove the person in the image who is standing next to the fence by the railway track.

*EditReward leaves the person unchanged (edit ignored); RewardClaw removes the person cleanly while preserving the scene.*

![case473](figures/473.jpg)
