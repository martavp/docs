# DTU Sophia hardware

## Compute

The Sophia HPC cluster consists of 516 computational nodes of which 484 are 128 GB RAM nodes and 32 are 256 GB RAM nodes. Each node is a powerful x86-64 computer, equipped with 32 physical cores (2 x sixteen-core AMD EPYC 7351).

The parameters are summarized in the following table:
| **Specs**                              |                                             |
| ------------------------------------------- | ------------------------------------------- |
| Primary purpose                             | High Performance Computing                  |
| Architecture of compute nodes               | x86-64                                      |
| Operating system                            | CentOS 7 Linux                            |
| Compute nodes in total                      | 516                                            |
| Processor                                   | 2 &times AMD EPYC 7351, 2.9 GHz, 16 cores        |
| RAM (484 nodes)                             | 128 GB, 4 GB per core, DDR4@2666 MHz         |
| RAM (32 nodes)                              | 256 GB, 8 GB per core, DDR4@2666 MHz         |
| Local disk drive                            | no                                          |
| Compute network / Topology                  | InfiniBand EDR / Fat tree                   |
| **In total**                                |                                             |
| Total theoretical peak performance  (Rpeak) | ~384 TFLOPS *(516 nodes &times 32 cores &times 2.9GHz &times 8 FLOP/cycle)*                                  |
| Total amount of RAM                         | 69 TB                                       |


## High-speed interconnect

The nodes are interlinked by InfiniBand and 10 Gbps Ethernet networks.

Sophia's high-speed, low-latency interconnect is Mellanox EDR (100Gbps) Infiniband.
Frontend-, compute-, and burst buffer nodes each have
Mellanox' [ConnectX-5](https://www.mellanox.com/page/products_dyn?product_family=258&mtag=connectx_5_vpi_card) adapter card installed.

| Switch system | Count |
| --------------|------:|
| [SB7700](http://www.mellanox.com/related-docs/prod_ib_switch_systems/pb_sb7700.pdf)        |    2 |
| [SB7790](http://www.mellanox.com/related-docs/prod_ib_switch_systems/pb_sb7790.pdf)        |   47 |


## Burst buffer

| Hardware product            | Count |
| ----------------------------|------:|
| [Dell R7425](https://i.dell.com/sites/doccontent/shared-content/data-sheets/en/Documents/PowerEdge-R7425-Spec-Sheet.pdf) w/NVMe front-bay |     2 |
| [Dell Express Flash PM1725a 1.6TB](https://www.samsung.com/semiconductor/global.semi.static/Brochure_Samsung_PM1725a_NVMe_SSD_1805.pdf)        | 16 |
| [Dell Express Flash PM1725b 1.6TB](http://image-us.samsung.com/SamsungUS/PIM/Samsung_1725b_Product.pdf)        | 4 | 

