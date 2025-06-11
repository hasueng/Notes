# READ README

## docs/devel/migration/virtio.rst
migration: VM을 다른 컴퓨터로 옮기는 과정
virtio 상태도 store했다가 load하는 과정이 필요함
load하는 과정에서 subsection을 확인하며 일련의 순서를 지키면서 load 되어야하며
그런 규칙을 설명하는 내용이 있음

## docs/devel/virtio-backends.rst
virtio device는 TYPE_VIRTIO_DEVICE를 사용한다.
VirtIOPCIProxy는 아마 legacy때문에 있을 것 :ex) virtio-blk-pci, 
"당신이 만드는 가상 장치에 vhost를 적용하고 싶나요? 두 가지 선택지가 있습니다.
    1. 둘 다 지원하려면: struct vhost_dev를 쓰고 vhost_dev_init()을 호출하세요. 가장 일반적입니다.
    2. vhost-user만 필요하다면: 제어 소켓으로 쓰이는 chardev를 잘 관리해야 합니다. 아니면 더 편하게 QOM 객체를 쓰는 방법도 고려해 보세요."


