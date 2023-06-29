# Создание контейнера seminar2.
    root@194-58-109-243:/# lxc-create -n seminar2 -t ubuntu
    Checking cache download in /var/cache/lxc/jammy/rootfs-amd64 ...
    Copy /var/cache/lxc/jammy/rootfs-amd64 to /var/lib/lxc/seminar2/rootfs ...
    Copying rootfs to /var/lib/lxc/seminar2/rootfs ...
    Generating locales (this might take a while)...
    en_US.UTF-8... done
    Generation complete.
    Creating SSH2 RSA key; this may take some time ...
    3072 SHA256:O7wnpUzuxOmTrtDouWrvYmzYiV1LwwfKeqte/xHLR+o root@194-58-109-243.cloudvps.regruhosting.ru (RSA)
    Creating SSH2 ECDSA key; this may take some time ...
    256 SHA256:PdNTBlJB0rNElJdm69BcAj4ZRPNWGa+9CWHGDzjwXDQ root@194-58-109-243.cloudvps.regruhosting.ru (ECDSA)
    Creating SSH2 ED25519 key; this may take some time ...
    256 SHA256:Q4p8CWLA3nn/6pKpku3kOlXxNGszCN/jHfX3pBTmLeI root@194-58-109-243.cloudvps.regruhosting.ru (ED25519)
    invoke-rc.d: could not determine current runlevel
    invoke-rc.d: policy-rc.d denied execution of start.

    Current default time zone: 'Etc/UTC'
    Local time is now:      Wed Jun 28 12:14:43 UTC 2023.
    Universal Time is now:  Wed Jun 28 12:14:43 UTC 2023.


    ##
    # The default user is 'ubuntu' with password 'ubuntu'!
    # Use the 'sudo' command to run tasks as root in the container.
    ##
# Запуск контейнера и проверка его работы.
    root@194-58-109-243:/# lxc-start -n seminar2
    root@194-58-109-243:/# lxc-attach -n seminar2
    root@seminar2:/# free -m
                total        used        free      shared  buff/cache   available
    Mem:             957          28         897           0          31         928
    Swap:              0           0           0
    root@seminar2:/# exit
    exit
    root@194-58-109-243:/# lxc-stop -n seminar2
# Ограничение контейнера 256 MB ОЗУ.
    root@194-58-109-243:/# echo "lxc.cgroup2.memory.max = 256M" >> var/lib/lxc/seminar2/config
    root@194-58-109-243:/# lxc-start -n seminar2
    root@194-58-109-243:/# lxc-attach -n seminar2
    root@seminar2:/# free -m
                total        used        free      shared  buff/cache   available
    Mem:             256          27         228           0           0         228
    Swap:              0           0           0
    root@seminar2:/# exit
    exit
    root@194-58-109-243:/# lxc-stop -n seminar2
# Настройка автозапуска и логирования.
    root@194-58-109-243:/# echo "lxc.start.auto = 1" >> var/lib/lxc/seminar2/config
    root@194-58-109-243:/# echo "lxc.log.level = 2" >> var/lib/lxc/seminar2/config
    root@194-58-109-243:/# echo "lxc.log.file = /usr/share/seminar2_log" >> var/lib/lxc/seminar2/config
# Результат после перезагрузки сервера.
    root@194-58-109-243:~# lxc-ls -f
    NAME     STATE   AUTOSTART GROUPS IPV4     IPV6 UNPRIVILEGED
    seminar2 RUNNING 1         -      10.0.3.6 -    false
    root@194-58-109-243:~# cat /usr/share/seminar2_log
    lxc seminar2 20230628130310.643 INFO     lxccontainer - lxccontainer.c:do_lxcapi_start:997 - Set process title to [lxc monitor] /var/lib/lxc seminar2
    lxc seminar2 20230628130310.649 INFO     lsm - lsm/lsm.c:lsm_init_static:38 - Initialized LSM security driver AppArmor
    lxc seminar2 20230628130310.655 INFO     seccomp - seccomp.c:parse_config_v2:807 - Processing "reject_force_umount  # comment this to allow umount -f;  not recommended"
    lxc seminar2 20230628130310.655 INFO     seccomp - seccomp.c:do_resolve_add_rule:524 - Set seccomp rule to reject force umounts
    lxc seminar2 20230628130310.655 INFO     seccomp - seccomp.c:do_resolve_add_rule:524 - Set seccomp rule to reject force umounts
    lxc seminar2 20230628130310.655 INFO     seccomp - seccomp.c:do_resolve_add_rule:524 - Set seccomp rule to reject force umounts
    lxc seminar2 20230628130310.655 INFO     seccomp - seccomp.c:parse_config_v2:807 - Processing "[all]"
    lxc seminar2 20230628130310.655 INFO     seccomp - seccomp.c:parse_config_v2:807 - Processing "kexec_load errno 1"
    lxc seminar2 20230628130310.655 INFO     seccomp - seccomp.c:do_resolve_add_rule:564 - Adding native rule for syscall[246:kexec_load] action[327681:errno] arch[0]
    lxc seminar2 20230628130310.655 INFO     seccomp - seccomp.c:do_resolve_add_rule:564 - Adding compat rule for syscall[246:kexec_load] action[327681:errno] arch[1073741827]
    lxc seminar2 20230628130310.655 INFO     seccomp - seccomp.c:do_resolve_add_rule:564 - Adding compat rule for syscall[246:kexec_load] action[327681:errno] arch[1073741886]
    lxc seminar2 20230628130310.655 INFO     seccomp - seccomp.c:parse_config_v2:807 - Processing "open_by_handle_at errno 1"
    lxc seminar2 20230628130310.655 INFO     seccomp - seccomp.c:do_resolve_add_rule:564 - Adding native rule for syscall[304:open_by_handle_at] action[327681:errno] arch[0]
    lxc seminar2 20230628130310.655 INFO     seccomp - seccomp.c:do_resolve_add_rule:564 - Adding compat rule for syscall[304:open_by_handle_at] action[327681:errno] arch[1073741827]
    lxc seminar2 20230628130310.655 INFO     seccomp - seccomp.c:do_resolve_add_rule:564 - Adding compat rule for syscall[304:open_by_handle_at] action[327681:errno] arch[1073741886]
    lxc seminar2 20230628130310.655 INFO     seccomp - seccomp.c:parse_config_v2:807 - Processing "init_module errno 1"
    lxc seminar2 20230628130310.655 INFO     seccomp - seccomp.c:do_resolve_add_rule:564 - Adding native rule for syscall[175:init_module] action[327681:errno] arch[0]
    lxc seminar2 20230628130310.655 INFO     seccomp - seccomp.c:do_resolve_add_rule:564 - Adding compat rule for syscall[175:init_module] action[327681:errno] arch[1073741827]
    lxc seminar2 20230628130310.655 INFO     seccomp - seccomp.c:do_resolve_add_rule:564 - Adding compat rule for syscall[175:init_module] action[327681:errno] arch[1073741886]
    lxc seminar2 20230628130310.655 INFO     seccomp - seccomp.c:parse_config_v2:807 - Processing "finit_module errno 1"
    lxc seminar2 20230628130310.655 INFO     seccomp - seccomp.c:do_resolve_add_rule:564 - Adding native rule for syscall[313:finit_module] action[327681:errno] arch[0]
    lxc seminar2 20230628130310.655 INFO     seccomp - seccomp.c:do_resolve_add_rule:564 - Adding compat rule for syscall[313:finit_module] action[327681:errno] arch[1073741827]
    lxc seminar2 20230628130310.655 INFO     seccomp - seccomp.c:do_resolve_add_rule:564 - Adding compat rule for syscall[313:finit_module] action[327681:errno] arch[1073741886]
    lxc seminar2 20230628130310.655 INFO     seccomp - seccomp.c:parse_config_v2:807 - Processing "delete_module errno 1"
    lxc seminar2 20230628130310.655 INFO     seccomp - seccomp.c:do_resolve_add_rule:564 - Adding native rule for syscall[176:delete_module] action[327681:errno] arch[0]
    lxc seminar2 20230628130310.655 INFO     seccomp - seccomp.c:do_resolve_add_rule:564 - Adding compat rule for syscall[176:delete_module] action[327681:errno] arch[1073741827]
    lxc seminar2 20230628130310.655 INFO     seccomp - seccomp.c:do_resolve_add_rule:564 - Adding compat rule for syscall[176:delete_module] action[327681:errno] arch[1073741886]
    lxc seminar2 20230628130310.655 INFO     seccomp - seccomp.c:parse_config_v2:1017 - Merging compat seccomp contexts into main context
    lxc seminar2 20230628130310.655 INFO     start - start.c:lxc_init:884 - Container "seminar2" is initialized
    lxc seminar2 20230628130310.655 INFO     cgfsng - cgroups/cgfsng.c:cgfsng_monitor_create:1029 - The monitor process uses "lxc.monitor.seminar2" as cgroup
    lxc seminar2 20230628130310.666 INFO     cgfsng - cgroups/cgfsng.c:cgfsng_payload_create:1137 - The container process uses "lxc.payload.seminar2" as inner and "lxc.payload.seminar2" as limit cgroup
    lxc seminar2 20230628130310.667 INFO     start - start.c:lxc_spawn:1765 - Cloned CLONE_NEWNS
    lxc seminar2 20230628130310.667 INFO     start - start.c:lxc_spawn:1765 - Cloned CLONE_NEWPID
    lxc seminar2 20230628130310.667 INFO     start - start.c:lxc_spawn:1765 - Cloned CLONE_NEWUTS
    lxc seminar2 20230628130310.667 INFO     start - start.c:lxc_spawn:1765 - Cloned CLONE_NEWIPC
    lxc seminar2 20230628130310.667 INFO     start - start.c:lxc_spawn:1765 - Cloned CLONE_NEWNET
    lxc seminar2 20230628130310.667 INFO     start - start.c:lxc_spawn:1765 - Cloned CLONE_NEWCGROUP
    lxc seminar2 20230628130310.667 WARN     cgfsng - cgroups/cgfsng.c:cgfsng_setup_limits_legacy:2767 - Invalid argument - Ignoring legacy cgroup limits on pure cgroup2 system
    lxc seminar2 20230628130310.667 INFO     cgfsng - cgroups/cgfsng.c:cgfsng_setup_limits:2863 - Limits for the unified cgroup hierarchy have been setup
    lxc seminar2 20230628130310.699 INFO     network - network.c:netdev_configure_server_veth:655 - Retrieved mtu 1500 from lxcbr0
    lxc seminar2 20230628130310.714 INFO     network - network.c:netdev_configure_server_veth:721 - Attached "vethcrn5R8" to bridge "lxcbr0"
    lxc seminar2 20230628130310.715 INFO     conf - conf.c:setup_utsname:875 - Set hostname to "seminar2"
    lxc seminar2 20230628130310.715 INFO     network - network.c:lxc_setup_network_in_child_namespaces:4019 - Finished setting up network devices with caller assigned names
    lxc seminar2 20230628130310.715 INFO     network - network.c:lxc_setup_network_in_child_namespaces:4035 - Finished setting up network devices with kernel assigned names
    lxc seminar2 20230628130310.715 INFO     conf - conf.c:mount_autodev:1219 - Preparing "/dev"
    lxc seminar2 20230628130310.716 INFO     conf - conf.c:mount_autodev:1280 - Prepared "/dev"
    lxc seminar2 20230628130310.718 INFO     conf - conf.c:run_script_argv:337 - Executing script "/usr/share/lxcfs/lxc.mount.hook" for container "seminar2", config section "lxc"
    lxc seminar2 20230628130310.775 INFO     conf - conf.c:lxc_fill_autodev:1317 - Populating "/dev"
    lxc seminar2 20230628130310.775 INFO     conf - conf.c:lxc_fill_autodev:1405 - Populated "/dev"
    lxc seminar2 20230628130310.775 INFO     conf - conf.c:lxc_transient_proc:3775 - Caller's PID is 1; /proc/self points to 1
    lxc seminar2 20230628130310.775 INFO     conf - conf.c:lxc_allocate_ttys:1109 - Finished creating 4 tty devices
    lxc seminar2 20230628130310.775 INFO     conf - conf.c:lxc_setup_ttys:1072 - Finished setting up 4 /dev/tty<N> device(s)
    lxc seminar2 20230628130310.776 INFO     conf - conf.c:setup_personality:1917 - Set personality to "0lx0"
    lxc seminar2 20230628130310.776 NOTICE   conf - conf.c:lxc_setup:4469 - The container "seminar2" is set up
    lxc seminar2 20230628130310.776 INFO     apparmor - lsm/apparmor.c:apparmor_process_label_set_at:1186 - Set AppArmor label to "lxc-container-default-cgns"
    lxc seminar2 20230628130310.776 INFO     apparmor - lsm/apparmor.c:apparmor_process_label_set:1231 - Changed AppArmor profile to lxc-container-default-cgns
    lxc seminar2 20230628130310.776 WARN     cgfsng - cgroups/cgfsng.c:cgfsng_setup_limits_legacy:2767 - Invalid argument - Ignoring legacy cgroup limits on pure cgroup2 system
    lxc seminar2 20230628130310.777 NOTICE   utils - utils.c:lxc_drop_groups:1368 - Dropped supplimentary groups
    lxc seminar2 20230628130310.777 NOTICE   start - start.c:start:2161 - Exec'ing "/sbin/init"
    lxc seminar2 20230628130310.788 NOTICE   start - start.c:post_start:2172 - Started "/sbin/init" with pid "796"