# SoftBodyLocomotion
For our Physics-based animation project, we combined the following two papers:
 - Soft Body Locomotion by Tan, Turk, and Liu (2012).
 - A Constraint-based Formulation of Stable Neo-Hookean Materials by Macklin and Müller (2021).
Soft bodies are made of a material that is easily pliable or deformable, such as gelatinous or squishy materials. In simulation, these bodies are a thriving research area, as the complex deformations present in such materials are difficult to represent and show in simulated models.

Locomotion is also a busy research area, as this allows artists to depend on physics for their own characters’ movements. However, as models are simulated, it is easy to move them with rigid skeletons that have straightforward controls for locomotion. Using soft bodies alone presents a unique method of moving. There are plenty of examples in nature of creatures using locomotion without a skeleton, such as worms, starfish, and jellyfish. How these creatures actually move depends on a mixture of contracting muscles, volume-preservation, and external friction.

The soft body locomotion paper above presents soft body characters, without skeletons, that simulate deformation by using a finite element method (FEM) simulation. Their implementation has the following block diagram:

Figure 1: Block diagram of the Soft Body Locomotion paper
We decided against using QPCC solver and replaced the locomotion controller with user input. We believe it is more interesting to a user to be able to manipulate the soft bodies and see the effect of the manipulation in generating locomotion. Here is an updated block diagram of our implementation:

Figure 2: Block diagram of our implementation
As we are using tetrahedral meshes for our project, we decided to augment the file format by introducing a third file with extension .mus. The file has a list of springs with an index. Each spring has indices to endpoints vertices of the spring. And a group id to group springs together to form muscles.
For the simulation part, we decided to use an extended position-based dynamics (XPBD) simulation instead of the FEM simulator in the original paper. For our XPBD simulation, we implemented two constraints: deviatoric and volume preserving. Our  implementation follows the XPBD soft bodies simulated in the second paper. The deviatoric constraints maintain the rest shape of each tetrahedron of the mesh while the volumetric constraints maintain the volume of all the tetrahedrons. We added a spring constraint to maintain the rest lengths of the muscles. 
The UI locomotion controller changes the rest lengths of groups of muscles in real-time. Any change of rest lengths will trigger the simulation constraints to update the tetrahedral meshes to the new rest shape and volume changes. These changes, connected with friction from the ground plane, generates locomotion of the soft bodies.
