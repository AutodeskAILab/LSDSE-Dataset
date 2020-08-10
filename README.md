
# Learning to Simulate and Design for Structural Engineering (LSDSE)
---------------------------------------------------------------------------

This repo stores the dataset used in the paper "Learning to Simulate and Design for Structural Engineering". The paper is published to ICML 2020 and here are the linkes to the [Paper](https://proceedings.icml.cc/static/paper_files/icml/2020/5700-Paper.pdf) and the [Video](https://www.youtube.com/watch?v=0KNW2GkmX2s).

Each json contains a list of 100 graphs where each graph represents a building strucuture. The graph is stored as a dictionary.
Assume we have a building structure with `N` bars(columns and beams) and `K` stories. Then, the corresponding structural graph has `N+1`(Ground).

---------------------------------------------------------------------------
## bar_nodes 
  - end point location `Nx6` : [x1, y1, z1, x2, y2, z2] (unit: ft) 
  - cross-section `Nx9` : One hot vector for cross-section. Please refer to 'cross_sections.json'. 
  -  if_beam `Nx1` : 1 if True, 0 if False 
  - if_boundary_beam `Nx1` : 1 if True, 0 if False
  - if_roof_beam `Nx1` : 1 if True, 0 if False
  - metal_deck_area_on_beam `Nx1` : Each metal deck panel sits across only two beams on its longer edges. The areas (unit: ft^2) are multiplied by the per-area load in surface load cases. Please refer to the Appendix of the paper for details of the load case setups. 
---------------------------------------------------------------------------
## edges
edges are stored with two lists: senders and receivers. The two lists of the same edge type should have the same length. Elements in the list are bar ID, the same ass index in the `bar_nodes` list.
***Column-column edge***
  - column_column_senders: upper-story column ID
  - column_column_receivers: lower-story column ID
 
***Beam-column edge***
  - beam_column_senders: beam ID
  - beam_column_receivers: column ID
 
***Column-graph edge***
  -  column_grounds_senders: First-story column ID
  -  column_grounds_receivers: `ground node ID = N`
---------------------------------------------------------------------------
## bar_id_in_stories
For a `K`-story building, this list contains `K+1` elements, where each element is a list containing all bar IDs in that story and the last element is the `ground node ID = N`.

---------------------------------------------------------------------------
## drift_ratio

Corresponding to the `bar_id_in_stories`, `drift_ratio` is a list containing `K+1` elements. Each elements contains four drift ratios [Ex_x, Ex_y, Ey_x, Ey_y] (x and y component of drift ratio under seismic Ex and Ey load cases). The last element for the story with a single ground node is `[0,0,0,0]`.
