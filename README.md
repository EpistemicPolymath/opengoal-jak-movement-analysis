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

### 1/21/2023

Back to digging into all of this again. What I also would like to try and break down and understand is under Game -> display - Tagret Marks. I feel that this in addition to Master marks can be the tools I need to create what I am envisioning, but I would of course have to breakdown how each of them works and see if I can create and alternative option that includes both with manipulations. 

Today, I want to look more into this function:

```
 (if *display-cam-master-marks*
    (debug-draw-spline (-> arg0 target-spline))
  )
```

In order for the master marks to display, this function uses the "debug-draw-spline* and attaches it to the target (Jak). Seeing how this function works would help put us in the right direction. Then if we can create similar, but mainpulated functions, maybe I can add functions for a whole new "marks" (E.g: Jak Movement Marks). 

Tracking Spline Function:
This has been an interesting finding. We might be able to simply use the tracking already built in and make some modifications from there.
data\decompiler\config\jak2\all-types.gc
```
(deftype tracking-spline (structure)
  ((point              tracking-point 32 :inline      :offset-assert 0) ;; guessed by decompiler
   (summed-len         float                  :offset-assert 1536)
   (free-point         int32                  :offset-assert 1540)
   (used-point         int32                  :offset-assert 1544)
   (partial-point      float                  :offset-assert 1548)
   (end-point          int32                  :offset-assert 1552)
   (next-to-last-point int32                  :offset-assert 1556)
   (max-move           float                  :offset-assert 1560)
   (sample-len         float                  :offset-assert 1564)
   (used-count         int32                  :offset-assert 1568)
   (old-position       vector         :inline :offset-assert 1584)
   (debug-old-position vector         :inline :offset-assert 1600)
   (debug-out-position vector         :inline :offset-assert 1616)
   (debug-last-point   int32                  :offset-assert 1632)
   )
  :method-count-assert 24
  :size-assert         #x664
  :flag-assert         #x1800000664
  (:methods
    (tracking-spline-method-9 (_type_) none 9)
    (tracking-spline-method-10 (_type_ vector) none 10)
    (debug-point-info (_type_ int) none 11)
    (debug-all-points (_type_) none 12)
    (tracking-spline-method-13 (_type_ int) none 13)
    (tracking-spline-method-14 (_type_ tracking-spline-sampler) none 14)
    (tracking-spline-method-15 (_type_) none 15)
    (tracking-spline-method-16 (_type_ float) none 16)
    (tracking-spline-method-17 (_type_ vector float float symbol) int 17)
    (tracking-spline-method-18 (_type_ float vector tracking-spline-sampler) vector 18)
    (tracking-spline-method-19 (_type_ float vector tracking-spline-sampler) vector 19)
    (tracking-spline-method-20 (_type_ vector int) none 20)
    (tracking-spline-method-21 (_type_ vector float float float) vector 21)
    (tracking-spline-method-22 (_type_ float) none 22)
    (debug-draw-spline (_type_) none 23)
    )
  )
```
Full debug-draw-spline function:
data\goal_src\jak2\engine\camera\cam-debug.gc

```
(defmethod debug-draw-spline tracking-spline ((obj tracking-spline))
  (let ((s5-0 (-> obj used-point)))
    (let ((s4-0 (-> obj point (-> obj used-point) next)))
      (let ((s3-0 (new 'stack-no-clear 'vector)))
        (when (!= s4-0 -134250495)
          (tracking-spline-method-19 obj 0.0 s3-0 (the-as tracking-spline-sampler #f))
          (camera-cross
            (new 'static 'vector :y 1024.0)
            (new 'static 'vector :z 1024.0)
            s3-0
            (new 'static 'vector4w :x #xff :w #x80)
            (the-as meters #x44800000)
            )
          (tracking-spline-method-19 obj (-> obj sample-len) s3-0 (the-as tracking-spline-sampler #f))
          (camera-cross
            (new 'static 'vector :y 1024.0)
            (new 'static 'vector :z 1024.0)
            s3-0
            (new 'static 'vector4w :x #xff :w #x80)
            (the-as meters #x44800000)
            )
          )
        )
      (while (!= s4-0 -134250495)
        (camera-line
          (the-as vector (-> obj point s5-0))
          (the-as vector (-> obj point s4-0))
          (new 'static 'vector4w :x #x80 :y #x80 :z #x80 :w #x80)
          )
        (set! s5-0 s4-0)
        (set! s4-0 (-> obj point s4-0 next))
        )
      )
    (if (!= s5-0 -134250495)
        (camera-cross
          (new 'static 'vector :y 1024.0)
          (new 'static 'vector :z 1024.0)
          (the-as vector (-> obj point s5-0))
          (new 'static 'vector4w :x #xff :w #x80)
          (the-as meters #x44800000)
          )
        )
    )
  (let ((s5-1 (new 'stack-no-clear 'vector)))
    (camera-line
      (-> obj debug-out-position)
      (-> obj debug-old-position)
      (new 'static 'vector4w :x #xff :y #xff :w #x80)
      )
    (vector-! s5-1 (-> obj debug-out-position) (-> obj debug-old-position))
    (tracking-spline-method-20 obj s5-1 (-> obj debug-last-point))
    (camera-line-rel (-> obj debug-old-position) s5-1 (new 'static 'vector4w :x #xff :z #xff :w #x80))
    )
  0
  (none)
  )
```

Notable files:
- data\goal_src\jak1\engine\target\logic-target.gc (All logic related to the target Jak. There are a lot of vector methods and drawings here that might be reuseable)
- data\goal_src\jak2\engine\camera\cam-debug.gc (All camera related debug logic and functions)
- data\decompiler\config\jak2\all-types.gc (Many functions are definied here and this a a good place to check what is what.)

How do I use functions, methods, variables from other files in: data\goal_src\jak1\engine\mods\define-custom-functions-here.gc

This is what I need. I am finding a lot of relevant functions, methods, variables, but I cannot interact with them in the test area. 


### 2/06/2023

Barg got basck to me with a lot of information so I am going to try and see what I can come up with today trying to decipher all of that information. 

I decided to create my own custom files to work with:
-   "mods/jak-movement-analysis.gc" ;; custom funtions file
-   "mods/jak-movement-analysis-defun.gc" ;; defun file
  
Then from here I can experiment and keep my code seaprate from other custom code. First there needs to be edits to game.gp so that I can use whatever functions, methods, etc that I will need. 

Barg corrected my mistake last time. I was referencing a Jak 2 function instead of a Jak 1 so I have corrected that now:

- Jak 2: data\goal_src\jak2\engine\camera\cam-debug.gc
- Jak 1: data\goal_src\jak1\engine\camera\cam-debug.gc

- Jak 2: data\decompiler\config\jak2\all-types.gc
- Jak 1: data\decompiler\config\all-types.gc

With the proper files collected, we can add them as dependencies to the game.gp:

```
(goal-src-sequence
 ;; prefix
 "engine/"
 :deps ("$OUT/obj/battlecontroller.o" "$OUT/obj/snow-bunny.o" "$OUT/obj/baby-spider.o" "$OUT/obj/sage-village3.o" "$OUT/obj/sage-finalboss.o" "$OUT/obj/assistant-citadel.o" "$OUT/obj/assistant-lavatube.o" "$OUT/obj/robocave-part.o" "$OUT/obj/driller-lurker.o" "$OUT/obj/training-part.o" "$OUT/obj/rolling-race-ring.o" "$OUT/obj/beach-part.o" "$OUT/obj/sculptor.o" "$OUT/obj/sunken-fish.o" "$OUT/obj/billy.o" "$OUT/obj/sidekick-human.o" "$OUT/obj/flying-lurker.o" "$OUT/obj/target-racer-h.o" "$OUT/obj/firecanyon-obs.o" "$OUT/obj/target-flut.o" "$OUT/obj/hud-classes-pc.o" "$OUT/obj/collide-reaction-racer.o" "$OUT/obj/plant-boss.o" "$OUT/obj/beach-obs.o" "$OUT/obj/sunken-elevator.o" "$OUT/obj/jungle-part.o" "$OUT/obj/sequence-a-village1.o" "$OUT/obj/ticky.o")
 "mods/mods-settings.gc"
 "mods/define-custom-functions-here.gc"
 "mods/put-custom-code-here.gc"
  ;; Jak Movement Analysis
  "mods/jak-movement-analysis.gc" ;; custom funtions file
  "mods/jak-movement-analysis-defun.gc" ;; defun file
)

```

Assumably I should be able to (mi) and run some of these functions. Everything is working correctly at this point. I ran out of time tonight to start actual experimentation, but I will do that the next time I work on the project. I will go into more detail about the information Barg shared last time as well. 

### Outside Resources I am reviewing 


- https://github.com/dryice10/ygoalstuff/blob/main/checkerpoint.gc
- https://github.com/dryice10/ygoalstuff/blob/main/measurer.gc

### Internal Resources I am reviewing: File Paths of Interest

- data\goal_src\jak1\examples\debug-draw-example.gc
- data\goal_src\jak1\game.gp




