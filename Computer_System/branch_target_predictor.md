# Branch target predictor


## Introduction
In computer architecture, a branch target predictor is the part of a processor that predicts the target of a taken conditional branch or an unconditional branch instruction before the target of the branch instruction is computed by the execution unit of the processor.

Branch target prediction is not the same as branch prediction which attempts to guess whether a conditional branch will be taken or not-taken.

In more parallel processor designs, as the instruction cache latency grows longer and the fetch width grows wider, branch target extraction becomes a bottleneck. The recurrence is:
 - Instruction cache fetches block of instructions
 - Instructions in block are scanned to identify branches
 - First predicted taken branch is identified
 - Target of that branch is computed
 - Instruction fetch restarts at branch target

In machines where this recurrence takes two cycles, the machine loses one full cycle of fetch after every predicted taken branch. As predicted branches happen every 10 instructions or so, this can force a substantial drop in fetch bandwidth. Some machines with longer instruction cache latencies would have an even larger loss. To ameliorate the loss, some machines implement branch target prediction: given the address of a branch, they predict the target of that branch. A refinement of the idea predicts the start of a sequential run of instructions given the address of the start of the previous sequential run of instructions.

This predictor reduces the recurrence above to:
 - Hash the address of the first instruction in a run
 - Fetch the prediction for the addresses of the targets of branches in that run of instructions
 - Select the address corresponding to the branch predicted taken

As the predictor RAM can be 5–10% of the size of the instruction cache, the fetch happens much faster than the instruction cache fetch, and so this recurrence is much faster. If it were not fast enough, it could be parallelized, by predicting target addresses of target branches.

## References:
- [Branch target predictor wiki](https://en.wikipedia.org/wiki/Branch_target_predictor)
