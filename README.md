# Collision-Aware-Needle-Path-Planning
Automated collision-aware needle path planning for image-guided interventions using CT/MRI and tumor segmentation. The pipeline generates, filters, and ranks safe insertion paths based on geometric and clinical constraints, recommends suitable ablation needles, and supports 3D visualization for validation.

Automated Optimisation-based Needle Path Planning– Project Workflow

1. DICOM Input
The pipeline begins with the input of patient-specific DICOM imaging data, acquired through CT scanning. These images provide the anatomical context required for planning a safe and clinically feasible percutaneous needle path.
2. Segmentation Using MOOSEz
The DICOM images are processed using MOOSEz for automated anatomical segmentation. This step extracts relevant structures such as organs and surrounding anatomy, which are later used for collision checking and safety assessment.
3. Tumour NIfTI (NII) Input
A pre-segmented tumour mask in NIfTI (.nii) format is provided as input. This mask represents the target lesion and serves as the primary reference for planning needle insertion.
4. Tumour Volume and Coordinate Computation
Using the tumour NIfTI mask, the system computes:
•	Tumour volume (based on voxel dimensions)
•	Tumour voxel coordinates in the CT reference frame
This information is essential for selecting appropriate needles and ensuring adequate coverage during the procedure.
5. Tumour Centroid Computation
The tumour centroid is calculated from the segmented mask and used as the target point for all candidate needle paths. This centroid represents the optimal geometric target for needle placement.
________________________________________
Optimisation-Based Sampling for Path Planning
6. Candidate Path Generation
An optimisation-based sampling approach is used to generate multiple candidate needle paths. Direction vectors are sampled in 3D space around the tumour centroid, and each direction is extended to compute a potential skin entry point, forming straight-line candidate paths. The skin is found by masking the body.

7. Collision Checking for Candidate Paths
Each candidate path is evaluated against segmented anatomical structures to detect potential intersections. Paths that collide with critical organs or restricted regions.
8. Identification of Collision-Free Paths
Only collision-free paths that satisfy hard clinical constraints (such as insertion angle and needle length limits) are retained for further evaluation. Unsafe or infeasible paths are discarded at this stage.
9. Path Ranking Using Length and Angle Constraints
The remaining feasible paths are ranked using a multi-objective cost function, primarily based on:
•	Needle path length
•	Needle insertion angle
Paths with lower cost (shorter length and clinically favourable angles) are prioritised as better candidates.
10. Needle Recommendation for the Procedure
Based on the ranked paths and the computed tumour volume, the system recommends the most suitable needle for the procedure.
________________________________________
Summary
This workflow combines medical imaging, anatomical segmentation, and optimisation-based sampling to produce clinically interpretable and safe needle path recommendations. The approach emphasises feasibility, safety, and practical deployment in image-guided percutaneous procedures.

Future Enhancements:

1.	COMPUTATION OF COST FUNCTION 
•	A cost function provides a numerical score to each feasible needle path based on multiple clinical factors such as path length, insertion angle, and safety.
•	Even when multiple paths are collision-free, some are clinically better than others. A cost function enables the objective comparison and ranking of paths, rather than selecting based on a single parameter. This feature is to be added to the top five ranking of the needle paths. 


2.	ADAPTIVE SAMPLING 
•	Adaptive sampling is a two-stage approach where initial global sampling identifies promising paths, followed by finer local sampling around the best candidates.
•	Uniform sampling may miss better paths. Adaptive sampling focuses computation on high-quality regions of the search space. This improves the path quality and reduces the wasted samples, and produces near-optimal solutions. 
3.	SOFT AND HARD CONSTRAINTS 
•	Hard constraints: Strict rules that must not be violated (e.g., organ collision, maximum angle limits).
•	Soft constraints: Preferences that influence ranking but do not reject paths outright (e.g., slightly larger angle, longer path) or could be the tissue.
Incorporating these constraints helps to maintain safety guarantees, and it enables fine-grained optimisation and reflects real clinical decision-making. 

4.	APPLYING PRODUCT OPERABILITY PREFERENCE
•	After the Ranking and generation of the top five paths based on length and angle constraints, we can apply product operability preference, which is a value in which the product Maxio is more comfortable or more feasible to operate. 

5.	INPUTTING DIGESTIVE SYSTEM FOR SEGMENTATION AND HARD CONSTRAINT 
•	Segmenting digestive organs (such as stomach, intestines, and bowel) and enforcing them as hard constraints during path planning.
•	Digestive organs are highly sensitive and mobile, and accidental puncture can lead to serious complications.
This improves patient safety and expands anatomical awareness of the planner, making the system more clinically complete.
6.	SAFETY MARGIN 
•	A safety margin is a buffer distance added around sensitive anatomy to account for Breathing motion, segmentation inaccuracies, needle deflection, and Registration errors. This helps to reduce the uncertainty.

<img width="735" height="407" alt="image" src="https://github.com/user-attachments/assets/d34b5a18-9981-4f28-909d-44e717a5629b" />


<img width="748" height="429" alt="image" src="https://github.com/user-attachments/assets/14b4bb72-1768-48a4-adff-e0b6da2241f1" />

<img width="559" height="389" alt="image" src="https://github.com/user-attachments/assets/3aa15b48-4c3c-4505-9bb7-6453ef86a8c9" />

<img width="592" height="453" alt="image" src="https://github.com/user-attachments/assets/2b751628-7fbf-451f-b61e-fbe6f154b03a" />

<img width="593" height="462" alt="image" src="https://github.com/user-attachments/assets/d8093b2e-a7e9-427c-85ca-a0dc68fb48e3" />

