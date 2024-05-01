# hlb-gpt-cli

CLI controllable version of hlb-gpt by tysam-code

[Fern](https://github.com/tysam-code)'s [hlb-gpt](https://github.com/tysam-code/hlb-gpt)
is a package for training transformers really, really fast, to high quality.

This package extends that one that one to be easily controllable over the CLI.

**Why would you want that?**

It makes ablations really easy. 

Just provide a whole bunch of settings over the command-line, and automatically run
highly repeatable experiments at different model scales, and log their results in high detail
to either wandb and/or a local .csv-file.

**Example**

How about we ablate the number of attention heads over different widths and depths?
You could run:

```bash
python v041 -lw --wandb_project test --seed 1000 --num_runs 5 --max_epochs 5 --depth 4 8 16 32 --width 192 384 --num_heads 1 3 6
```

What does that do?

- `-lw`: Equivalent to `--log_csv --log_wandb`. Log both to a .csv-file (by default, "results_041.csv"), and to a wandb-project.
- `--wandb_project test`: Log to the wandb-project "test"
- `--seed 1000`: Manually set the seed. 
- `--num_runs 5`: For each setting, do train 5 times. Each run is initialized with a different seed, starting with 
    the one given in `--seed`. 
    So here, in each settings, the corresponding 5 runs would have the seeds [1000, 1001, 1002, 1003, 1004],
    making them highly comparable and repeatable.
- `--max_epochs 5`: Training runs for 5 epochs. An eval is guaranteed at the end.
- `--depth 4 8 16 --width 192 384 --num_heads 1 3 6`: Every combination of these values represents one setting, and will be run for 5 runs.
    If `width % num_runs != 0` in some settings, that setting won't be run.

As you can see, it is easy to run very large and highly repeatable ablations of different settings.
I tried to make the code easy to extend, so that you can easily ablate your own settings.
More on that below.
