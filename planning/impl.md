# IMPLEMENTATION GUIDE

The purpose of this guide is to reduce the context carried by claude and by the human reviewer at the moment of review. The smaller steps between checkins breaks up the agentic workflow, reducing the effect of small bugs by catching them early, before more implementation is built on top.

All steps to be completed in order. PHASE 2 is a series of steps that may be repeated as a group.

## PLAN (PHASE 1)

### 1. PLAN (STEP 1)

Requires: milestone.

1.1 Read principles.md to plan according to the project priorities. May read design.md and requirements.md for when additional context required (note where the milestone deviates from design and requirements. If deviation is explicitly explained in the milestone, create plan with deviation explanation. If deviation is not explicitly acknowledged in the plan, ask user for clarifications.).

1.2 Use the planning skill to write a plan doc for the milestone. Write the plan to `docs/superpowers/plans/`.

1.3 Outline the tasks required to complete the milestone for the user.

1.4 Wait for user to approve the plan before moving to Step 2.

## IMPLEMENTATION (PHASE 2)

For each task in the plan perform the following steps. Do not begin a new task until all steps have been completed for the current task and approved by user.

### 2. IMPLEMENT (STEP 2)

Requires: plan.

2.1 Read the principles doc to refresh the principles with which the plan should be implemented.

2.2 Implement the task from the plan.

2.3 Wait for user to approve changes before moving foward with Step 3.

### 3. CRITICAL REVIEW (STEP 3)

3. 1 Review the implementation critically. What problems have been introduced? How should the implementation be changed to better achieve the deliverable of the milestone, accomplish the goal of the task, and align with principles.md?

3.2 Present the problem and solution for each problem identified. Restart Phase 2 with Step 2, IMPLEMENT.

3.3 If no problems identified, wait for user to approve review and changes before moving forward with Step 4.

#### 4. UPDATE DOCUMENTATION (STEP 4)

4.1 Update design, requirements, roadmap, readme, and walkthrough according to the implemented changes from the task plan.

4.2 Wait for user approval. 

4.3 Only if user says to do so, create commits for implemented changes.

4.4 Wait for user to approve changes before moving foward with the next task. If all tasks completed, wait for user to approve changes before moving foward with Step 5.

## PR REVIEW (PHASE 3)

Once user has approved Step 5 (meaning all tasks in milestone plan are complete), perform the following.

### 5. PR REVIEW (STEP 5)

5.1 If branch and PR have not already been created, push branch to repo and create PR. Otherwise, push changes to preexisting branch and PR.

5.2 If user prompts, read the comments and reviews from the PR, assess, then explain the problem and proposed solutions to the user.

5.3 Wait for user approval before implementing fixes. User may ask for changes to the fixes. Once user approves all fixes, proceed to 5.4 for each fix.

5.4 If the fix is broad enough to merit its own plan, return to step 1 for the fix specifically and complete the implementation for the fix before moving on with the milestone. If the fix does not require its own plan, return to Phase 2 Step 2 for the fix.

5.5 Wait for user approval before moving to 5.6.

5.6 Iterate step 5 until no comments, fixes, or changes are needed on the PR.

5.7 Ask user before removing all plans created during the IMPLEMENTATION GUIDE process.

