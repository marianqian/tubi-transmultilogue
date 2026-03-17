# TransMultilogue
10617 Final Project: Improving Multimodal Sentiment Analysis Using Multilogue-Net and Transformer Architecture  

Final report linked [here](10617_final_report.pdf).  
Our model weights can be found in their respective Jupyter Notebooks.  

---

## File Structure 

### [transformer](transformer)
Folder containing the Jupyter Notebook for our **modified Multilogue-Net + transformer architecture**.  
- Notebook contains instructions on how to run the model.  
- Uses code from [Multilogue-Net](https://github.com/amanshenoy/multilogue-net).  
- We modified [model.py](https://github.com/marianqian/tubi-transmultilogue/blob/main/transformer/model.py#L184) (lines 184–284), training script, and requirements file.  
- Full theory and design rationale explained in [report](10617_final_report.pdf).  

### [baseline](baseline) 
Folder containing the Jupyter Notebook for the baseline model.  
- Baseline taken from [MultiBench examples](https://github.com/pliang279/MultiBench/tree/main/examples).  
- We modified one supervised training script to output MAE metrics for training and validation.  
- Full theory explained in [report](10617_final_report.pdf).  

---

## Project Overview
TransMultilogue improves multimodal sentiment analysis by fusing **Multilogue-Net GRUs** with a **pretrained transformer** to better leverage text knowledge and modality-specific temporal modeling.  

- Each modality passes through GRUs for **emotion, context, and state**.  
- **Emotion vectors** from all modalities are concatenated and passed through a pretrained transformer (Hugging Face Reformer, ~3M parameters).  
- Transformer hidden state is combined with original emotion state, then passed through an MLP for regression output.  

---

## Motivations / Why Previous Work Was Useful

### Multilogue-Net
- Separate GRUs for **emotion, context, and state** improve temporal modeling.  
- Attention over context vectors allows cross-modal interactions.  
- **Motivation for our project:** Preserve GRU structure while integrating pretrained transformer knowledge.

### UniMSE
- Uses pretrained T5 transformer and modality fusion for sentiment prediction.  
- Ablation studies show performance improvements from pretrained text knowledge.  
- **Motivation:** Incorporate transformer knowledge without full resource-heavy fusion.  

---

## Marian Contribution
This project was completed with my partner Michelle. I list our contributions separately below.

My contributions (Marian): 
- Modified a **small part of the Gated-Multilogue-Net class** in [model.py](https://github.com/marianqian/tubi-transmultilogue/blob/main/transformer/model.py#L184-L284).  
  - Added reasoning and documented **key design choices** for improved modality fusion and context representation.  
  - Implemented the **TransMultilogue pipeline** combining GRUs and pretrained transformer embeddings.  
- Ran experiments for the transformer implementation.  

Michelle's contributions: 
- Modifying baseline scripts and running the baseline experiments

---

## Key Design Choices
- **GRU-specific modality representation:** Maintains temporal structure and captures emotion, context, and state independently.  
- **Transformer-based cross-modal fusion:** Efficiently leverages pretrained textual knowledge.  
- **Concatenation of emotion vectors as transformer input:** Lightweight alternative to full modality fusion.  
- **MLP for regression:** Maps fused features to target sentiment outputs.  
- **Gating modification:** Balances modalities for improved context representation; fully documented with rationale.  

---

## Training Process
- Used same hyperparameters as Multilogue-Net for baseline comparability.  
- Experiments limited by resource constraints; ideally, more hyperparameter trials would improve robustness.  
- Evaluated on standard multimodal sentiment datasets (e.g., MOSEI).  

---

## Results & Analysis
- Our method achieved a **mean absolute error (MAE) of 0.617**, compared to **0.59 for Multilogue-Net**.  
- Possible reasons for the performance gap:  
  1. **Smaller transformer model:** We used the Reformer (~3M parameters), pretrained only on *Crime and Punishment*, limiting textual understanding.  
  2. **Modality fusion method:** Concatenating features before passing through the transformer may not allow sufficient interaction between modalities compared to UniMSE’s deeper pretrained modality fusion.  
  3. **Transformer hidden state usage:** Only interactions at the current timestep were used; previous hidden states were not carried forward. Incorporating prior hidden states could better capture context and temporal modality interactions.  

**Future steps:**  
- Use a larger transformer model with more comprehensive pretraining.  
- Implement full pretrained modality fusion as described in UniMSE.  
- Include transformer hidden states across timesteps to better capture temporal context.  

---

## Future Improvements
- Experiment with **larger transformer models** for better utilization of pretrained knowledge.  
- Explore **full pretrained modality fusion** by injecting audio/video features into deeper transformer layers.  
- Conduct additional **hyperparameter trials** for optimization and reproducibility.  

---

## References
- [Multilogue-Net: Multimodal GRU Network for Sentiment Analysis](https://github.com/amanshenoy/multilogue-net)  
- [UniMSE: Pretrained Transformer-based Multimodal Sentiment Analysis](https://aclanthology.org/2022.emnlp-main.534)  
