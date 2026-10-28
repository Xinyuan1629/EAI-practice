# Further evaluation: single training seed

Evaluation seeds repeat evaluation of fixed policies; they are not independent training seeds.
Contact regions are checkpoint-specific. Counts are gated repeated contact steps; bouts are uninterrupted finger-region contacts.
Wilson intervals describe episode sampling only; they do not quantify training-seed variability.

| Job | Successes / episodes | Success rate | Mean terminal step | Contact steps / bouts |
|---|---:|---:|---:|---:|
| baseline_c2000_combined_s10001 | 0/384 | 0.00% | 298.8 | 644/520 |
| baseline_c2000_combined_s10002 | 0/384 | 0.00% | 298.7 | 914/752 |
| baseline_c2000_friction_high_s10001 | 0/384 | 0.00% | 298.9 | 715/584 |
| baseline_c2000_friction_high_s10002 | 0/384 | 0.00% | 299.0 | 1011/832 |
| baseline_c2000_friction_low_s10001 | 0/384 | 0.00% | 298.3 | 357/275 |
| baseline_c2000_friction_low_s10002 | 0/384 | 0.00% | 298.2 | 543/440 |
| baseline_c2000_nominal_s10001 | 0/384 | 0.00% | 299.0 | 643/515 |
| baseline_c2000_nominal_s10002 | 0/384 | 0.00% | 299.0 | 856/707 |
| baseline_c2000_xy2mm_s10001 | 0/384 | 0.00% | 299.0 | 608/480 |
| baseline_c2000_xy2mm_s10002 | 0/384 | 0.00% | 298.9 | 954/765 |
| baseline_c2000_xy4mm_s10001 | 0/384 | 0.00% | 298.8 | 611/483 |
| baseline_c2000_xy4mm_s10002 | 0/384 | 0.00% | 298.8 | 874/698 |
| baseline_c2000_yaw2deg_s10001 | 0/384 | 0.00% | 298.5 | 643/522 |
| baseline_c2000_yaw2deg_s10002 | 0/384 | 0.00% | 298.6 | 886/714 |
| contactexplorer_c1000_nominal_s10001 | 0/384 | 0.00% | 296.7 | 46913/6639 |
| contactexplorer_c1000_nominal_s10002 | 0/384 | 0.00% | 296.8 | 67467/8518 |
| contactexplorer_c1250_nominal_s10001 | 373/384 | 97.14% | 217.1 | 50827/5530 |
| contactexplorer_c1250_nominal_s10002 | 359/384 | 93.49% | 222.6 | 62604/5915 |
| contactexplorer_c2000_combined_s10001 | 326/384 | 84.90% | 226.1 | 52713/4509 |
| contactexplorer_c2000_combined_s10002 | 328/384 | 85.42% | 227.9 | 63704/5132 |
| contactexplorer_c2000_friction_high_s10001 | 239/384 | 62.24% | 261.1 | 61797/6185 |
| contactexplorer_c2000_friction_high_s10002 | 238/384 | 61.98% | 260.7 | 60617/6724 |
| contactexplorer_c2000_friction_low_s10001 | 255/384 | 66.41% | 238.4 | 47271/3984 |
| contactexplorer_c2000_friction_low_s10002 | 309/384 | 80.47% | 225.7 | 55039/4565 |
| contactexplorer_c2000_nominal_s10001 | 333/384 | 86.72% | 225.6 | 54564/4413 |
| contactexplorer_c2000_nominal_s10002 | 312/384 | 81.25% | 231.5 | 61434/4984 |
| contactexplorer_c2000_xy2mm_s10001 | 315/384 | 82.03% | 227.6 | 53753/4586 |
| contactexplorer_c2000_xy2mm_s10002 | 332/384 | 86.46% | 227.0 | 64940/5276 |
| contactexplorer_c2000_xy4mm_s10001 | 314/384 | 81.77% | 229.8 | 55692/4606 |
| contactexplorer_c2000_xy4mm_s10002 | 316/384 | 82.29% | 228.6 | 64453/5053 |
| contactexplorer_c2000_yaw2deg_s10001 | 323/384 | 84.11% | 226.9 | 53466/4641 |
| contactexplorer_c2000_yaw2deg_s10002 | 323/384 | 84.11% | 229.4 | 63853/5236 |
