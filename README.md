# Poly's Jak Movement Analaysis Project

## The Goal

I want to use OpenGOAL to visually track Jak's movement timings (line drawings - visuals) as well as mathematically calculate the distance Jak moved in order to determine distance variations between movement, combinations of movement, and more. 

I have dyscalculia, so being able to have something drawn in addition to the math would be incredibly helpful. I think this game be done with OpenGOAL. I will see what I can do on my own and try to pull from others work as well. I will share all other content I am looking at as I work on this here and update it accordingly.


## Progress

- Currently reviewing resources and learning OpenGOAL as much as I can. 

### 12/04/2022

#### Learned GoalC Commands
(li) - connects the compiler to the running game
(reload) - restarts it (updates it)
(mi) - Builds all of the code in the game

#### Formatting and Syntax
(inspect *game-info*)
(inspect *target*)
(-> *game-info* money)
(_function_)
*object*
(define _variables_ value)

#### Useful variables
Jak's Location in XYZ
(inspect (->(-> *target* root)trans))
(inspect (->(-> *target* root)trans x))
(inspect (->(-> *target* root)trans y))
(inspect (->(-> *target* root)trans z))

(meters 1.0) (Distance value to compare from)

Code Blocks
 (_set_! (-> *game-info* money) 32.0 )
(set! (-> *game-info* fuel) (+ (-> *game-info* fuel) 1.0) )
(inspect (->(-> *target* root)trans))

### Outside Resources I am reviewing 


- https://github.com/dryice10/ygoalstuff/blob/main/checkerpoint.gc
- https://github.com/dryice10/ygoalstuff/blob/main/measurer.gc

### Internal Resources I am reviewing: File Paths of Interest

- data\goal_src\jak1\examples\debug-draw-example.gc
- data\goal_src\jak1\game.gp




