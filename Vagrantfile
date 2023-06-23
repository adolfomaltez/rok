  # Vagrantfile: 
  # Create 3x PXE nodes for elemental

  # Variables
  NODES = 3
  MEMORY = 4200
  CPU = 2


  Vagrant.configure("2") do |config|

      config.vm.define "node-01" do |vm|
        vm.vm.provider :libvirt do |libvirt|
          libvirt.cpus = CPU
          libvirt.memory = MEMORY
          libvirt.graphics_ip = '0.0.0.0'
          libvirt.video_type = 'qxl'
          libvirt.storage :file, :size => '30G', :type => "raw"
          libvirt.storage :file, :device => :cdrom, :path => "/tmp/elemental.iso", :type => "raw"
          libvirt.loader = "/usr/share/OVMF/OVMF_CODE.fd"
          libvirt.nvram = "/usr/share/OVMF/OVMF_VARS.fd"
          libvirt.storage_pool_name = "vmdisks"
          libvirt.boot "hd"
          libvirt.boot "cdrom"
          libvirt.tpm_model = "tpm-crb"
          libvirt.tpm_type = "emulator"
          libvirt.tpm_version = "2.0"
          libvirt.qemuargs :value => "-smbios"
          libvirt.qemuargs :value => "type=1,serial=0001"
        end
      end

      config.vm.define "node-02" do |vm|
        vm.vm.provider :libvirt do |libvirt|
          libvirt.cpus = CPU
          libvirt.memory = MEMORY
          libvirt.graphics_ip = '0.0.0.0'
          libvirt.video_type = 'qxl'
          libvirt.storage :file, :size => '30G', :type => "raw"
          libvirt.storage :file, :device => :cdrom, :path => "/tmp/elemental.iso", :type => "raw"
          libvirt.loader = "/usr/share/OVMF/OVMF_CODE.fd"
          libvirt.nvram = "/usr/share/OVMF/OVMF_VARS.fd"
          libvirt.storage_pool_name = "vmdisks"
          libvirt.boot "hd"
          libvirt.boot "cdrom"
          libvirt.tpm_model = "tpm-crb"
          libvirt.tpm_type = "emulator"
          libvirt.tpm_version = "2.0"
          libvirt.qemuargs :value => "-smbios"
          libvirt.qemuargs :value => "type=1,serial=0002"
        end
      end

      config.vm.define "node-03" do |vm|
        vm.vm.provider :libvirt do |libvirt|
          libvirt.cpus = CPU
          libvirt.memory = MEMORY
          libvirt.graphics_ip = '0.0.0.0'
          libvirt.video_type = 'qxl'
          libvirt.storage :file, :size => '30G', :type => "raw"
          libvirt.storage :file, :device => :cdrom, :path => "/tmp/elemental.iso", :type => "raw"
          libvirt.loader = "/usr/share/OVMF/OVMF_CODE.fd"
          libvirt.nvram = "/usr/share/OVMF/OVMF_VARS.fd"
          libvirt.storage_pool_name = "vmdisks"
          libvirt.boot "hd"
          libvirt.boot "cdrom"
          libvirt.tpm_model = "tpm-crb"
          libvirt.tpm_type = "emulator"
          libvirt.tpm_version = "2.0"
          libvirt.qemuargs :value => "-smbios"
          libvirt.qemuargs :value => "type=1,serial=0003"
        end
      end

  end