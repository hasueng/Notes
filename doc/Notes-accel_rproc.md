# Notes accel_rproc.c snps npx/vpx driver

## Contents
- snps_accel_rproc_probe
    - firmware 존재 확인
    - snsp_accel_rproc_ops 준비
        - prepare, start, stop, da_to_va, get_boot_addr, load, sanity_check
    - arcsync_get_device_by_phandle : DT에서phandle을 통해 다른 디바이스 노드를 참조하고, 해당 디바이스를 커널 코드에서 찾는 과정
    - snps_accel_rproc_init_ctrl_with_arcsync_fn : ctrl function 등록 
        - aproc->ctrl.fn에 arcsync_func:aproc->ctrl.dev 대입
        - arcsync_fn : arcsync->funcs by arcsync_get_ctrl_fn
    - snps_accel_rproc_of_get_mem : firmware를 위한 메모리 영역 할당  

- snps_accel_rproc_start
    - echo start > /sys/class/remoteproc/remoteproc0/state
    - 호출 순서
        1. state_store in remoteproc_sysfs.c : 특정 buf의 값을 읽어서 start일때 동작
        2. rproc_boot in remoteproc_core.c : request_firmware를 통해서 load
            - rproc_fw_boot
            - rproc_start
            - rproc->ops->start(rproc); : start = snps_accel_rproc_start, aproc=rproc
            - aproc->data->start_core(aproc) : start_core = arcsync_start_core , setup all cores
            - fn->start(ctrl, aproc->cluster_id, aproc->core_id[i]); in arcsync_start_core
                - 개요: 이 start는 무엇인가?: start가 등록되는 과정 살펴보기
                - *fn=&aproc->ctrl.fn : snps_accel_rproc_ctrl_fn에 start가 있음. : struct with pointers for control function
                - ( struct snps_accel_rproc *aproc = rproc->priv ) ? 아직 파악 못함
                - ( snps_accel_rproc_ctrl : ctrl이고 fn 포함 )
                - arcsync_probe in snps_arcsync.c : arcsync->funcs= &arcsync_ctrl, struct arcsync_func arcsync_ctrl
                - arcsync->funcs = &arcsync_ctrl
                - arcsync_ctrl에 .start=arcsync_start
                - snps_accel_rproc_init_ctrl_with_arcsync_fn: 결국 이 함수를 통해서 aproc->ctrl.fn의 start에 arcsync_start가 등록됨 
            - arcsync_start : writel(1, arcsync->regs + reg_offset), 레지스터 셋팅을 통해서 코어 시작
