We thank all reviewers, the SPC, and the AC for their constructive feedback and recognition of the research question and controlled experiments. We clarify the evidence, scope, and limitations below. Our findings concern the evaluated intrinsic injection mechanisms in JEPA-style shared-latent predictors, rather than physics-informed world models generally.

**Reviewer yt5w**

**Q1: The hypothesis requires verification beyond simple PhyWorld scenes.**

**A:** The submission includes direct training on the more complex Physion++ benchmark, separately from zero-shot Physion evaluation (Sec. 4.2; Appendix B, Table 3). We evaluate representatives of slot, consistency, and probe supervision over three seeds. For example, baseline versus slot nMSE is 0.171±0.079 versus 0.616±0.112 on mass_collision, 0.069±0.013 versus 0.424±0.122 on mass_dominoes, and 0.122±0.002 versus 0.440±0.073 on mass_waterpush. All evaluated injection variants are worse in every seed on these three scenarios; friction_platform remains unresolved because its baseline is unstable.

These results extend the performance observation beyond simple circular-object motion. However, Physion++ remains simulated, and its experiments do not establish the full redundancy mechanism in complex scenes. We will distinguish these claims and improve dataset visualization.

**Q2: Latent-capacity ablations and successful injection directions are missing.**

**A:** We agree that the controlled scan at D=192 does not establish capacity-independent behavior. We will make this restriction explicit. The frozen-DINOv2 replication changes the visual backbone but does not replace a latent-dimension sweep.

The paper also contains a positive injection result: velocity-plus-slot improves parabola nMSE from 0.122±0.007 to 0.096±0.007, with improvement in every evaluated seed. This suggests a useful boundary: dynamics-matched supervision can help, although the same configuration does not improve the other domains. Object-centric representations and structured transition operators remain promising directions outside our tested family.

**Q3: Cosine similarity does not justify “nearly exact” prediction or the introductory inference.**

**A:** We agree. Position availability is supported by probes against ground-truth position, whereas cosine similarity measures latent alignment. High cosine alone establishes neither accurate physical prediction nor preservation of state during rollout. We will remove “nearly exact” and separate these observations.

**Reviewer JQ68**

**Q1: Redundancy is not isolated from loss imbalance or optimization interference.**

**A:** We agree that the experiments do not establish redundancy as the sole cause of unsuccessful injection. Table 3 supports a narrower observation: non-slot position correlations are 0.92/0.85/0.79 across the three domains, compared with 0.92/0.85/0.78 for the full latent. The interventions separately demonstrate that the injected channel can affect prediction.

Auxiliary-loss dominance and reduced effective dimensionality provide evidence consistent with optimization interference, but scalar loss ratios do not establish gradient conflict. We will revise the causal wording accordingly. Furthermore, the prediction-coordinate weight w and auxiliary coefficient λ are different parameters: Figure 4 sweeps w and does not rule out benefits from smaller λ. This remains an unresolved alternative explanation under the submitted experiments.

**Q2: Does redundancy extend beyond position and across model architectures?**

**A:** Our strongest mechanistic evidence concerns the supervised position slot. The velocity experiments examine performance and decodability, but do not establish equivalent causal redundancy for velocity, acceleration, or contact properties. We will distinguish the nine-variant performance study from the representative position-slot mechanism analysis. Likewise, the DINOv2 replication broadens visual-backbone coverage within the predictive framework; it does not establish generality to reconstruction-based or object-centric world models.

**Q3: Model-specific latent metrics and three-seed overlap weaken the negative conclusion.**

**A:** We acknowledge both limitations. Appendix B provides a pixel-space cross-check for the uniform probe/slot comparison: baseline, probe-only, slot-plus-reweighting, and their combination obtain PSNRs of 20.41/19.83/21.30/20.02 dB alongside nMSEs of 0.131/0.167/0.114/0.125. Rankings agree, but this single-seed subset does not validate the entire grid.

We will also replace “within noise” with “unresolved across the evaluated seeds.” The descriptive breakdown is 15 configurations worse in every seed, 11 unresolved under our range-based criterion, and one better in every seed. Range overlap is not an equivalence test, and we will not interpret these 11 configurations as proven null effects.

**Reviewer vM6P**

**Q1: Slot perturbations establish sensitivity, not necessity or contribution to accurate prediction.**

**A:** We agree. “Load-bearing” denotes causal channel use, not indispensability or improved natural-distribution accuracy. Appendix D reads intervention effects from the non-slot dimensions rather than the overwritten channel, avoiding trivial readback. Steering, patching, and Jacobians support channel use, but slot-only interventions introduce disagreement between encodings and do not establish beneficial use during ordinary operation.

Appendix D also compares position-carrying black-box directions through steering, replacement, and INLP. The replacement comparison passes both controls only for LeWM/uniform, where approximately 100 replaced dimensions match the influence of the two-dimensional slot. We explicitly exclude other cells with failed controls. We will foreground these boundaries.

**Q2: How were auxiliary weights selected, and does free-rollout improvement exclude injection-specific problems?**

**A:** The main scan uses λ=1; the submission examines increased auxiliary weighting and a separate coordinate-weight sweep. It does not provide comprehensive validation-based optimization of every mechanism, particularly below λ=1. We will state this limitation explicitly.

The free-rollout positive control establishes that the shared pipeline can detect substantial improvement. It does not exclude injection-specific implementation errors, objective imbalance, or unsuitable hyperparameters. We will narrow this claim.

**Q3: Terminology and presentation are difficult to follow.**

**A:** We will define “slot” as designated latent coordinates, consistently distinguish initialization regime from rollout-training protocol, and replace broad “factorial” descriptions with an explicit experiment-coverage matrix. We will also clarify the slot construction:

[AUTHOR CHECK: State the exact encoding, supervision/replacement, and rollout operations; reconcile “hard pinning” with residual supervision error.]

**Reviewer BTwH**

**Q1: Single-configuration ablations do not establish conclusions across all variants.**

**A:** Correct. The scratch/fine-tuning comparison evaluates strict gravity plus a fixed slot; it does not establish that scratch training cannot rescue probe or consistency supervision. Likewise, Figure 4 concerns coordinate reweighting for the position slot. We will restrict both conclusions to these configurations and make experimental coverage explicit.

**Q2: Should architectural conclusions be scoped more narrowly, and could a robust PIWM encoder resolve OOD failure?**

**A:** Yes. Our findings concern JEPA-style predictive models with the tested visual backbones. They do not establish the same mechanism for VAE-based or discrete-latent predictors. PIWM changes multiple architectural factors, so its comparison does not isolate redundancy. A more OOD-robust PIWM encoder could improve performance; our experiment establishes a limitation of the evaluated port, not an unavoidable trade-off.

**Q3: Why does velocity-plus-slot succeed on parabola?**

**A:** Appendix B reports the domain-dependent effect. A plausible interpretation is that explicit velocity matches parabola’s evolving state, while uniform velocity is constant and collision velocity changes discontinuously. This interpretation is consistent with the performance pattern but is not causally established. We will present the positive result prominently and qualify its explanation.

**Reviewer NMij**

**Q1: Is architectural change necessary to remove redundancy?**

**A:** Our evidence does not establish necessity. Appendix D includes inference-time INLP: eliminating linearly decodable position requires removing 146–154 of 190 dimensions. However, random controls remove substantially different variance, preventing clean causal attribution. This does not substitute for training-time suppression. We will remove the architectural-necessity claim.

**Q2: Is the interaction between injection and training protocol measured?**

**A:** No. All primary injection variants use free-rollout training, so the scan measures their effect relative to that baseline. The scratch/fine-tuning experiment addresses initialization, not teacher-forcing versus free-rollout interaction. We will clarify this distinction and avoid extending the H=8 collision scan to other training horizons.

**Q3: Does Physion corroborate the conclusion when trained models do not exceed the random prior?**

**A:** We agree it does not corroborate a learned redundancy mechanism. We will present Physion zero-shot results as a transfer limitation, separately from Physion++ direct-training evidence.

**Additional implementation clarification**

[AUTHOR CHECK: Specify whether strict gravity fixes the true acceleration or learns a constant, including direction, units, normalization, and Δt; correct “true gravity supplied” if necessary.]

We will also distinguish real-frame probes from predicted-rollout probes and audit the reported interval calculations. These revisions preserve the empirical findings while aligning each mechanistic and generalization claim with its supporting evidence.