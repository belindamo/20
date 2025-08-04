# Literature Review

# Literature Review

## Overview

This literature review examines the current state of transformer model compression for edge deployment, focusing on three primary techniques: knowledge distillation, quantization, and architectural optimization. The review follows CS197 methodology, analyzing each paper through the lens of problem identification, prior assumptions, novel insights, technical approaches, evaluation methods, and broader impact.

## Foundational Transformer Architecture

### Attention Is All You Need (Vaswani et al., 2017)

**Problem**: Sequential computation in RNNs and CNNs limits parallelization and makes it difficult to model long-range dependencies effectively.

**Prior Assumptions**: RNNs and CNNs were considered necessary for sequence transduction tasks; sequential computation was accepted as an inherent limitation.

**Insight**: Self-attention mechanism can replace recurrence and convolution entirely, allowing for massive parallelization while effectively modeling dependencies regardless of distance.

**Technical Approach**: Multi-head self-attention mechanism with positional encodings, eliminating recurrent and convolutional layers entirely.

**Evaluation**: Demonstrated superior performance on machine translation tasks (WMT 2014 En-De and En-Fr) with significantly reduced training time.

**Impact**: Foundational paper that revolutionized NLP and computer vision, enabling the development of BERT, GPT, and Vision Transformers.

### BERT: Pre-training of Deep Bidirectional Transformers (Devlin et al., 2019)

**Problem**: Unidirectional language models limit the understanding of context in natural language understanding tasks.

**Prior Assumptions**: Left-to-right or right-to-left context was sufficient for language understanding; task-specific architectures were necessary for different NLP tasks.

**Insight**: Bidirectional pre-training using masked language modeling enables deeper understanding of context, and a single pre-trained model can be fine-tuned for multiple tasks.

**Technical Approach**: Masked Language Model (MLM) and Next Sentence Prediction (NSP) objectives during pre-training, followed by task-specific fine-tuning.

**Evaluation**: Achieved new state-of-the-art results on 11 NLP tasks including GLUE, SQuAD v1.1, and SQuAD v2.0.

**Impact**: Established the pre-train then fine-tune paradigm that dominates modern NLP, but highlighted the computational challenges of large transformer models.

## Knowledge Distillation Approaches

### DistilBERT: A Distilled Version of BERT (Sanh et al., 2019)

**Problem**: BERT's large size (110M parameters) makes deployment on resource-constrained devices impractical.

**Prior Assumptions**: Model compression required task-specific distillation; pre-training compression was not explored.

**Insight**: Knowledge distillation can be applied during pre-training to create smaller general-purpose models that retain most of the teacher's capabilities.

**Technical Approach**: Triple loss combining language modeling, distillation (KL divergence), and cosine embedding losses; student model has half the layers of BERT-base.

**Evaluation**: Achieves 97% of BERT-base performance on GLUE while being 40% smaller and 60% faster.

**Impact**: Pioneered pre-training distillation and demonstrated that smaller models could achieve near-teacher performance across multiple tasks.

### TinyBERT: Distilling BERT for Natural Language Understanding (Jiao et al., 2020)

**Problem**: Existing distillation methods don't fully leverage the rich information in transformer architectures.

**Prior Assumptions**: Simple output-level distillation was sufficient; intermediate layer information was not crucial for knowledge transfer.

**Insight**: Transformer-specific distillation targeting attention matrices and hidden states enables more effective knowledge transfer than output-only distillation.

**Technical Approach**: Two-stage distillation (general and task-specific) with attention-based distillation, hidden state distillation, and embedding layer distillation.

**Evaluation**: TinyBERT₄ achieves 96.8% of BERT-base performance while being 7.5× smaller and 9.4× faster.

**Impact**: Established attention-based distillation as a key technique and demonstrated extreme compression ratios with minimal performance loss.

### EI-BERT: Edge Ultra-lite BERT Framework (Wang et al., 2025)

**Problem**: Existing compressed models still too large for mobile edge applications requiring strict privacy and real-time constraints.

**Prior Assumptions**: Teacher-student relationship is unidirectional; students cannot provide feedback to improve knowledge transfer.

**Insight**: Cross-distillation where teacher models understand student perspectives enables more efficient knowledge transfer and ultra-compact models.

**Technical Approach**: Hard token pruning, cross-distillation with parameter integration, and quantization in a comprehensive pipeline.

**Evaluation**: Achieves 1.91MB model size (smallest for NLU tasks) while maintaining competitive performance; deployed on 8.4M daily active devices.

**Impact**: Demonstrates practical ultra-compact model deployment at scale and introduces bidirectional knowledge transfer concepts.

## Quantization Techniques

### DopQ-ViT: Distribution-Friendly and Outlier-Aware Post-Training Quantization (Yang et al., 2024)

**Problem**: Vision Transformers suffer significant performance degradation under quantization due to power-law distribution of post-Softmax activations and outlier sensitivity.

**Prior Assumptions**: Standard quantization methods designed for CNNs work equally well for Vision Transformers; uniform quantization is optimal.

**Insight**: ViT-specific quantization requires distribution-aware methods (Tan Quantizer) and outlier-robust scaling factors to preserve performance.

**Technical Approach**: TanQ for power-law distribution preservation and MAD-guided Optimal Scaling Factor (MOSF) for outlier handling.

**Evaluation**: Outperforms previous PTQ methods on ImageNet classification and COCO detection across various ViT architectures.

**Impact**: Addresses ViT-specific quantization challenges and provides practical solutions for vision transformer deployment.

### Towards Next-Level Post-Training Quantization (Jeon et al., 2024)

**Problem**: Existing PTQ methods are computationally expensive and don't consider inter-layer dependencies in attention modules.

**Prior Assumptions**: Layer-wise quantization is sufficient; attention module dependencies can be ignored for efficiency.

**Insight**: Attention-wise reconstruction while maintaining layer-wise efficiency balances accuracy and computational cost.

**Technical Approach**: AESPA algorithm performing layer-wise quantization with attention-wise reconstruction targeting cross-layer dependencies.

**Evaluation**: Achieves better accuracy-efficiency trade-offs on various language models compared to existing PTQ methods.

**Impact**: Provides practical quantization solution balancing computational efficiency with transformer-specific architectural considerations.

## Edge Deployment and Optimization

### Constrained Edge AI Deployment: Fine-Tuning vs. Distillation (Sander et al., 2024)

**Problem**: Optimal compression strategy for edge deployment under connectivity constraints is unclear.

**Prior Assumptions**: Fine-tuning with labeled data is superior to distillation methods; compression method choice doesn't significantly impact edge performance.

**Insight**: Self-distillation with KL-divergence matches or exceeds cross-entropy fine-tuning performance, particularly valuable for intermittent connectivity scenarios.

**Technical Approach**: Comparative analysis of L2P fine-tuning vs. L2P self-distillation using MLP-only pruning on OLMo2-7B-SFT.

**Evaluation**: Demonstrated equivalent or superior performance of distillation on CommonsenseQA under identical pruning schedules.

**Impact**: Provides practical guidance for edge deployment strategies and highlights the value of label-free compression methods.

### Vision-Language Models for Edge Networks (Sharshar et al., 2025)

**Problem**: Vision-Language Models (VLMs) are too resource-intensive for edge deployment despite their multimodal capabilities.

**Prior Assumptions**: VLM compression requires specialized techniques different from unimodal models; edge deployment of VLMs is impractical.

**Insight**: Standard compression techniques (pruning, quantization, distillation) can be effectively adapted for VLMs with appropriate modifications.

**Technical Approach**: Comprehensive survey and evaluation of pruning, quantization, knowledge distillation, and specialized hardware solutions for VLMs.

**Evaluation**: Analysis across healthcare, environmental monitoring, and autonomous systems applications.

**Impact**: Establishes feasibility of edge VLM deployment and provides framework for multimodal model compression.

## Research Gaps and Limitations

### Identified Assumptions Across Literature

1. **Scale-Performance Trade-off**: Current literature assumes linear relationship between model size and performance, but recent work suggests more complex relationships.

2. **Architecture Agnostic Compression**: Many methods assume compression techniques work equally across different transformer variants (BERT, ViT, T5, etc.).

3. **Single-Metric Optimization**: Focus on accuracy preservation while other metrics (latency, memory, energy) receive less systematic attention.

4. **Static Deployment Constraints**: Most work assumes fixed hardware constraints rather than adaptive deployment scenarios.

### Critical Research Gaps

1. **Holistic Compression**: Limited work on combining multiple compression techniques optimally for specific edge constraints.

2. **Dynamic Compression**: Lack of adaptive compression methods that adjust to runtime conditions and requirements.

3. **Cross-Modal Compression**: Insufficient exploration of compression techniques specifically designed for multimodal transformers.

4. **Hardware-Aware Co-Design**: Gap between compression algorithm development and actual hardware deployment considerations.

## Synthesis and Future Directions

The literature reveals a clear evolution from basic model compression to sophisticated, transformer-aware optimization techniques. The field has progressed through several phases:

1. **Foundation Era** (2017-2019): Establishment of transformer architecture and recognition of computational challenges.

2. **Compression Pioneering** (2019-2021): Initial distillation and quantization approaches adapted from CNN techniques.

3. **Architecture-Aware Optimization** (2022-2024): Development of transformer-specific compression methods considering attention mechanisms and architectural peculiarities.

4. **Edge-Focused Deployment** (2024-2025): Practical deployment solutions addressing real-world constraints and multi-objective optimization.

### Emerging Bit Flip Opportunities

The comprehensive literature analysis reveals a potential **bit flip** in the fundamental assumption about compression methodology:

**Current Assumption**: Compression should preserve the original model's behavior as closely as possible.

**Potential Flip**: Compression should optimize for task-specific requirements rather than model fidelity, potentially accepting larger deviations from the original model if they improve deployment metrics.

This flip suggests exploring compression techniques that prioritize deployment efficiency over model similarity, potentially unlocking new compression ratios while maintaining task performance.

---

**References**:
- Vaswani, A., et al. (2017). Attention is all you need. NeurIPS.
- Devlin, J., et al. (2019). BERT: Pre-training of deep bidirectional transformers for language understanding. NAACL.
- Sanh, V., et al. (2019). DistilBERT, a distilled version of BERT: smaller, faster, cheaper and lighter. arXiv preprint.
- Jiao, X., et al. (2020). TinyBERT: Distilling BERT for natural language understanding. EMNLP Findings.
- Wang, M., et al. (2025). Put Teacher in Student's Shoes: Cross-Distillation for Ultra-compact Model Compression Framework. arXiv preprint.
- Yang, L., et al. (2024). DopQ-ViT: Towards Distribution-Friendly and Outlier-Aware Post-Training Quantization for Vision Transformers. arXiv preprint.
- Sander, J., et al. (2024). Constrained Edge AI Deployment: Fine-Tuning vs. Distillation Analysis. arXiv preprint.
- Additional 18+ papers from comprehensive search covering edge deployment, quantization methods, and compression surveys.

---
*Enhanced using CS197 Research Methodology by The Research Company AI Agent*


---
*This section is being enhanced by The Research Company AI Agent*
