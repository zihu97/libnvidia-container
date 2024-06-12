-----------------------cli-----------------------
可执行文件，解析用例命令
解析argp，找到属于commands中的哪一类

cli/libnvc.[c/h] 将nvc_*等接口封装成libnvc.*的形式调用

cli/info.c
info_command->libnvc.init->nvc_init->driver_init->driver_init_1_svc->nvml_dl->nvmlInit_v2
            ->libnvc.driver_info_new->nvc_driver_info_new
            ->libnvc.device_info_new->nvc_device_info_new

cli/list.c
list_command

cli/configure.c
configure_command->libnvc.init
                 ->libnvc.container_new->nvc_container_new->get_device_cgroup_version->nvcgo_get_device_cgroup_version_1_svc->GetDeviceCGroupVersion
                                                          ->find_device_cgroup_path
                 ->libnvc.driver_info_new
                 ->libnvc.device_info_new
                 ->new_devices
                 ->select_devices
                 ->select_mig_config_devices
                 ->select_mig_monitor_devices
                 ->dsl_evaluate
                 ->libnvc.driver_mount
                 ->libnvc.device_mount
                 ->libnvc.mig_device_access_caps_mount
                 ->libnvc.mig_config_global_caps_mount
                 ->libnvc.device_mig_caps_mount
                 ->libnvc.mig_monitor_global_caps_mount
                 ->libnvc.imex_channel_mount
                 ->libnvc.ldcache_update

-----------------------nvcgo-----------------------



------------------------nvc------------------------
nvc.c 定义了诸如nvc_init等nvc_*等接口

rpc.[c/h] 用Unix套接字自定义了rpc通信接口

注册rpc服务：
driver_init--->driver_program_1(由nvc_rpc.x定义的，应该是xdr文件)--->driver_get_rm_version_res DRIVER_GET_RM_VERSION(ptr_t) = 3;
DRIVER_GET_RM_VERSION--->driver_get_rm_version_1/driver_get_rm_version_1_svc
客户端调用：
driver_get_rm_version--->driver_get_rm_version_1--->DRIVER_GET_RM_VERSION
服务端调用：
DRIVER_GET_RM_VERSION--->driver_get_rm_version_1_svc--->nvmlSystemGetDriverVersion

xdr_*为XDR的序列化和反序列化函数，由RPC工具（如RPCGEN）根据数据结构自动生成，读取的是.x文件

utils.[c/h] 一些工具函数