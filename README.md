# README — A3

# Partners: Leila Erbay and Richard Gao



##################### Part2 Question 1 ############################

init_cgroup(int num_settings) :

- function to allocate space for control_cgroup* 

- num_settings is the number of settings that will be necessary for the particular cgroup

- eg. cpu_cgroup has 3 settings: cpu.shares, self_to_task, and NULL

- initialized cgroup_control* for each of the cgroups: cpu, cpuset, pids, memory, blkio

- blkio we copied what was originally in cgroups[0] related to blkio

- case H: set config.hostname to the value associated with -H

- case C: fill the settings related to cpu cgroup and place in the next NULL inside cgroups**

- case s: fill the settings related to cpuset cgroup and place in the next NULL inside cgroups**

note order of placing self_to_task after cpuset.cpus and cpuset.mems is important

- case p: fill the settings related to pids cgroup and place in the next NULL inside cgroups**

- case M: fill the settings related to pids cgroup and place in the next NULL inside cgroups**

- case r: find the next NULL of blkio settings and place related settings there and replace cgroups[0] with the edited blkio cgroup

- case w: find the next NULL of blkio settings and place related settings there and replace cgroups[0] with the edited blkio cgroup 

##################### Part2 Question 2 ############################

- create the stack for the clone function and set the necessary flags for the 

##################### Part2 Question 3 ############################

- set the child’s root to the new_root

- syscall(SYS_pivot_root,new_root, put_old);

##################### Part2 Question 4 ############################

- set up child capabilities

- drop_caps is the caps we are selecting to drop

- we drop the selected capabilities from the ambient set

- clear the capabilities from the inheritable set

- set the cleared set back to the process 

##################### Part2 Question 5 ############################

- setup_syscall_filters

- setup default behavior of filtering context

- set filters on ptrace, mbind, migrate_pages, move_pages, unshare, clone, chmod - S_ISUID, chmod - S_ISGID

//clone on CLONE_NEWUSER

    /*by definition of the manpage, clone's flags are it's 3 arg but according to Shabir's

    contact with the author of the libseccomp library, we may need to use SCMP_A0 with clone due

    to different architecture... odd*/

    /*filter_set_status = seccomp_rule_add(seccomp_ctx, SCMP_FAIL, SCMP_SYS(clone),

                                         1, SCMP_A2(SCMP_CMP_MASKED_EQ, CLONE_NEWUSER, CLONE_NEWUSER) );*/

    filter_set_status = seccomp_rule_add(seccomp_ctx, SCMP_FAIL, SCMP_SYS(clone),

                                     1, SCMP_A0(SCMP_CMP_MASKED_EQ, CLONE_NEWUSER, CLONE_NEWUSER) );

- Note that we initially set the filter on clone using SCMP_A2 but according to a small FB post Shabir (TA) mentioned that the

 use of SCMP_A0 with clone may be due to architecture differences.

##################### ADDITIONAL FILES ############################

- memHog.c: tests our cgroup settings if it properly kills when it exceeds memory limit

NOTE: stalls when flags -r and -w were used to create the container…. so don’t use -r and/or -w when creating the container 

if you are planning to run memHog.c

- test_syscall.c: tests if our restrictions on the specific syscalls hold — specifically tests chmod and clone

##################### INTERESTING CASES ############################

- for a period of time, we were unable to create a new docker container

- another time, when we ran SNR_CONTAINER with bare minimum flags the program would stall at

if (execve(config-&gt;argv[0], config-&gt;argv, NULL))

    {

        fprintf(stderr, "invocation to execve() failed! %m.\n");

        return -1;

}

- this situation we were not able to solve, but if you run into this problem, wait a bit of time and rerun the docker container as well as the program or try to create another docker container and follow the necessary steps. 

 
