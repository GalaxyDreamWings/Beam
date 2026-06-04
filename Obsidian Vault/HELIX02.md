## HELIX 02
Each system operates at its natural timescale. System 2 (S2) reasons slowly about goals: interpreting scenes, understanding language, and sequencing behaviors. System 1 (S1) thinks fast, translating perception into full‑body joint targets at 200 Hz. System 0 (S0) executes at 1 kHz, handling balance, contact, and coordination across the entire body. Together, they form a tightly integrated hierarchy from pixels to torque.
### System 0: Human-like Whole-body Control from Human Data
S0 is a foundation model for human-like whole-body control: a learned prior over how people move while maintaining balance and stability. It is the backbone of physical embodiment for Helix 02: while higher layers reason about tasks and plans, S0 ensures every motion is executed smoothly, safely, and stably.
**Rather than engineering separate reward functions for walking, turning, crouching, or reaching, S0 learns to track human motion directly from a large and diverse corpus of movement data**. In learning to reproduce these motions, the policy learns how to coordinate forces, adjust posture, and maintain balance across the full range of behaviors needed for enabling general loco-manipulation.
Training data: Over 1,000 hours of joint‑level retargeted human motion data.
Architecture: A 10M‑parameter neural network that takes full‑body joint state and base motion as input and outputs joint‑level actuator commands at 1 kHz.
Simulation training: S0 is trained entirely in simulation across more than 200,000 parallel environments with extensive domain randomization, enabling direct transfer to real robots and generalization across the fleet.
### System 1: "All sensors in, all joints out" Visuomotor Policy
In the original Helix, S1 controlled the upper body and read from joint state and images. In Helix 02, it connects to all sensors, and controls the entire robot.

- **Inputs**: Head cameras, palm cameras, fingertip tactile sensors, and full‑body proprioception.
- **Outputs**: Complete joint-level control of the entire robot - legs, torso, head, arms, wrists, and individual fingers.

This **pixels‑to‑whole‑body** architecture allows S1 to reason about the complete state of the robot and environment as a single coupled system. The palm cameras and tactile sensors are new hardware capabilities from [Figure 03](https://www.figure.ai/news/introducing-figure-03). This is the first time we've demonstrated neural network policies that depend on these modalities.
**Palm cameras** provide in‑hand visual feedback when objects are occluded from the head camera. **Tactile sensors** embedded in each fingertip detect forces as small as three grams - sensitive enough to feel a paperclip - enabling contact‑aware, force‑modulated grasping. These sensing modalities enable Helix to unlock the full dexterity potential of five-fingered hands, tackling intricate manipulation tasks which demand the fine motor control of multi-fingered grasping.
S1 remains a transformer conditioned on System 2 latents, but now produces full‑body joint targets that S0 tracks at kHz rates.
### System 2: Scene Understanding and Language
System 2 remains the semantic reasoning layer: processing scenes, understanding language, and producing latent goals for S1. 
S2 doesn't need to plan low-level footsteps or specify how to coordinate arms and legs. It produces a sequence of semantic latents that S1 interprets into motor commands, which S0 executes.