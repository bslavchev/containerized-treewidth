# Containerized treewidth solvers

This repository contains scripts to build **(unofficial)** Docker images of the treewidth solvers associated with the [PACE challenge on treewidth](https://github.com/PACE-challenge/Treewidth). Each DOCKERFILE creates a minimal Alpine Linux environment inside a Docker container, adds the respective solver's dependencies and builds them if necessary, clones the solver from its GitHub repository, builds it, and creates entrypoints that allow the solver to be executed from the host system.

## Implemented solvers and solver entrypoints:
* [treedec](https://gitlab.com/freetdi/treedec) ([DockerHub image](https://hub.docker.com/r/containerizepace/treewidth_treedec)): --ex17
* [meiji](https://github.com/TCS-Meiji/PACE2017-TrackA) ([DockerHub image](https://hub.docker.com/r/containerizepace/treewidth_meiji)) (Tamaki, H., 2019): tw-exact
* [jdrasil](https://github.com/maxbannach/Jdrasil) ([DockerHub image](https://hub.docker.com/r/containerizepace/treewidth_jdrasil)) (Bannach et al., 2017): jdrasil.Exact
* [twalgor](https://github.com/twalgor/tw) ([DockerHub image](https://hub.docker.com/r/containerizepace/treewidth_twalgor)) (Tamaki, 2022): twalgor.main.ExactTW

## Usage
[Docker](https://www.docker.com/) is a prerequisite to both build and execute any of the solvers. In order to build a solver, execute the `build.sh` script in the respective folder using a UNIX shell.

In order to use a solver without building the image yourself, use the `docker pull` command listed on the image's DockerHub page.

To execute a solver:
```docker run -it --rm -v $PWD:/data containerizepace/[SOLVER] sh run.sh [IN] [TIMEOUT] [OUT]```

* `[SOLVER]` is one of the solvers from the above list
* `[IN]` is the relative path to the [PACE .gr file](https://github.com/PACE-challenge/Treewidth#input-format) that represents the graph you want the solver to run on 
* `[TIMEOUT]` is the number of seconds the solver is allowed to run before terminating
* `[OUT]` is the relative path to the [PACE-formatted](https://github.com/PACE-challenge/Treewidth#output-format) desired output (default file extension `.td`)


## An important note on solver entrypoints

Currently, only one entrypoint per solver is implemented - the one that most closely reflects the respective solver's submission to the exact track of PACE 2017. In the future, more entrypoints will be made available.

In the meantime, in order to build the image for a different entrypoint, you need to modify the respective `run.sh` file, then re-run `build.sh` to re-build the image. Please refer to each solver's GitHub repository for a reference of the entrypoints they implement.

## Credits
My approach to containerizing these solvers is based on the approach taken in the [ConSequences](https://github.com/TheoryInPractice/ConSequences)  framework (Dumitrescu et al., 2018).

## Bibliography
* Bannach, M., Berndt, S., & Ehlers, T. (2017). Jdrasil: A modular library for computing tree decompositions. In 16th International Symposium on Experimental Algorithms (SEA 2017). Schloss Dagstuhl-Leibniz-Zentrum fuer Informatik. https://doi.org/10.4230/LIPIcs.SEA.2017.28
* Dumitrescu, E. F., Fisher, A. L., Goodrich, T. D., Humble, T. S., Sullivan, B. D., & Wright, A. L. (2018). Benchmarking treewidth as a practical component of tensor network simulations. PloS one, 13(12), e0207827. https://doi.org/10.1371/journal.pone.0207827
* Tamaki, H. (2019). Positive-instance driven dynamic programming for treewidth. Journal of Combinatorial Optimization, 37(4), 1283-1311. https://doi.org/10.48550/arXiv.1704.05286
* Tamaki, H. (2022). Heuristic computation of exact treewidth. arXiv preprint arXiv:2202.07793.
