
# GenEx: Generating an Explorable World

## Core Idea:
  * Generates explorable 3D environments from a single RGB image.
  * Enables AI agents to explore, interact, and learn within these worlds.
  * Aims to advance embodied AI by bridging generative and physically grounded worlds.

## Key Components:
   * Imaginative World:
       * Dynamically generates 3D-consistent environments.
       *  Maintains coherence even during long-range exploration.
       *  Grounded in the physical world through data from physics engines.
       *  Utilizes panoramic representations (cubemaps, equirectangular panoramas).
   * Embodied Agent:
        *  Interacts with the generated world.
        * Enhanced by GPTs for decision-making.
        * Refines understanding and makes informed choices based on simulations.
  
## Process:
   * World Initialization:
        *  Generates a 360° panorama from a single image and text description.
        * Uses a text-to-panorama diffusion model.
        *  Conditioned on both image and text input.
   * World Transition:
        * Models environment change as an action-driven panoramic video generation.
        * Transforms previous panorama into a new view based on agent movements.
        * Uses sphere rotation and panoramic video generation.
        * Implements spherical-consistent learning for smooth imagery.

## Exploration:
   * Exploration Policy:
        * Decides agent actions.
        *  Actions include rotation angle and forward distance.
   * Exploration Modes:
        *   Interactive Exploration: User-controlled movement.
        *   GPT-Assisted Free Exploration: GPT-4o acts as a "pilot" to maximize generation quality.
        *   Goal-Driven Navigation: GPT for high-level planning based on navigation instructions.
        
## Advancing Embodied AI:
   *  Imagination-Augmented Policy:
        *  Gathers "imagined observations" within generated world.
        *  Simulates outcomes to make more informed decisions.
        * Selects an action maximizing policy based on imagined observations.
   * Multi-Agent Scenarios:
        * Agents explore each other's positions and environments.
        * Improves collaborative planning and decision-making.

## Applications:
   * High Generation Quality: Demonstrated through metrics like FVD, SSIM, LPIPS, and PSNR.
   * Exploration Loop Consistency (IELC): Robust long-range exploration.
   * Generating Bird's-Eye Views: Top-down maps from single panoramic image.
   *  3D Consistency: Superior multi-view video generation.
   * Active 3D Mapping: Uses tools like DUSt3R.
   *   Embodied Decision Making:
        *  Vision without imagination can be misleading.
        *  GenEx enhances human and GPT decision accuracy with visual "imagination".
   * Potential Uses:
        *  Real-world navigation.
        *  Interactive gaming and VR/AR.
        *  Improved embodied AI.
        

## Key Technical Details:
 * Data: Uses physics engines like Unreal Engine and Unity to gather diverse training data.
 * Panoramic representations: Captures complete 360-degree views, maintain global context.
 * Spherical-Consistent Learning (SCL): Ensures continuity across all viewing directions.
 * Math: Appendix focuses on spherical math of 2D equirectangular panorama as a 3D spherical representation.
 
## Challenges and Future Directions:
*   Bridging imaginative and real-world environments.
*   Sim-to-real adaptation, sensor integration, dynamic conditions.
*   Addressing ethical safeguards.

## Related Work:
 * Builds upon work in single-image 3D modeling and video generation.
 * Unites these domains with physically grounded data for world exploration.
 * Extends earlier work by adding world initialization from a single image.
 * Complements industrial progress by other groups (e.g., WorldLabs, DeepMind).

## Conclusion:
*   GenEx creates immersive and interactive worlds from a single image.
*  Enables exploration, 3D mapping, and better decision-making.
*  Supports multi-agent interactions.
* A step towards more advanced embodied AI.
