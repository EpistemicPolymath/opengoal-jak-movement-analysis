# Poly's Jak Movement Analaysis Project

## The Goal

I want to use OpenGOAL to visually track Jak's movement timings (line drawings - visuals) as well as mathematically calculate the distance Jak moved in order to determine distance variations between movement, combinations of movement, and more. 

I have dyscalculia, so being able to have something drawn in addition to the math would be incredibly helpful. I think this game be done with OpenGOAL. I will see what I can do on my own and try to pull from others work as well. I will share all other content I am looking at as I work on this here and update it accordingly.


## Progress

- Currently reviewing resources and learning OpenGOAL as much as I can. 

### 12/04/2022

#### Learned GoalC Commands
- (lt) - connects the compiler to the running game
- (reload) - restarts it (updates it)
- (mi) - Builds all of the code in the game

#### Formatting and Syntax
- (inspect *game-info*)
- (inspect *target*)
- (-> *game-info* money)
- (_function_)
- *object*
- (define _variables_ value)

#### Useful variables
Jak's Location in XYZ
- (inspect (->(-> *target* root)trans))
- (inspect (->(-> *target* root)trans x))
- (inspect (->(-> *target* root)trans y))
- (inspect (->(-> *target* root)trans z))

(meters 1.0) (Distance value to compare from)

Code Blocks
- (_set_! (-> *game-info* money) 32.0 )
- (set! (-> *game-info* fuel) (+ (-> *game-info* fuel) 1.0) )
- (inspect (->(-> *target* root)trans))

### 1/9/2023

I am starting up working on this project again. I have contacted Barg to see if he can help with the project. I have explored the code for Barg's suggestion of "Camera Master Marks." This feature does seem to be something I can manipulate to bring my mod idea into reality, especially in addition to what I found with Zed. I will try to save all file locations where I can better explore the usage of the "master marks" in the OpenGOAL code base.

#### Useful Functions and Files:

- *display-cam-master-marks* ;data\goal_src\jak1\engine\camera\cam-debug.gc
- (define-extern *display-cam-master-marks* symbol) ;data\decompiler\config\all-types.gc
- (define *display-cam-master-marks* #f) ; data\goal_src\jak1\engine\game\main-h.gc

Function of interest:
```
 (if *display-cam-master-marks*
    (debug-draw-spline (-> arg0 target-spline))
  )
```

Seems I might be able to use (debug-draw-spline) and attach it to the target (Jak) target-spline. With some mofications I can potentially add distance calculations (with the information I found with Zed) and adjust the visuals of these marks for easy viewing and comparison and call it something else altogether even. 
I will dive more into these functions and how they work and maybe get an answer.

### Outside Resources I am reviewing 


- https://github.com/dryice10/ygoalstuff/blob/main/checkerpoint.gc
- https://github.com/dryice10/ygoalstuff/blob/main/measurer.gc

### Internal Resources I am reviewing: File Paths of Interest

- data\goal_src\jak1\examples\debug-draw-example.gc
- data\goal_src\jak1\game.gp




