# Tóm tắt kết quả thực nghiệm Uplift

- Thời gian chạy UTC: 2026-09-30T13:51:23.386497+00:00 – 2026-09-30T14:06:06.373953+00:00 (14.72 phút)
- Chế độ: `colab_t4_full`; seed: 42
- Dữ liệu gốc: 13,979,592 quan sát; train/validation/test logic: 9,786,802/2,097,019/2,095,771
- Mẫu thực tế: train 9,786,802; validation 2,097,019; test 2,095,771
- Thuật toán cơ sở: LightGBM; Causal Forest: causalml CausalRandomForestRegressor
- Xác suất phân bổ vào nhóm treatment theo thiết kế: 0.850000
- Điểm hòa được phá bằng SplitMix64 từ `row_id` và seed 42, có thể tái lập và độc lập với treatment/outcome; quy tắc chỉ đổi thứ tự giữa các score bằng nhau.

## Phiên bản thư viện

| package | version |
| --- | --- |
| pandas | 2.2.3 |
| numpy | 2.1.3 |
| scikit-learn | 1.6.1 |
| lightgbm | 4.7.0 |
| causalml | 0.17.0 |
| matplotlib | 3.10.0 |
| seaborn | 0.13.2 |
| pyarrow | 23.0.1 |
| joblib | 1.6.0 |

## Siêu tham số và ứng viên

Các ứng viên S-Learner đều được huấn luyện trên toàn bộ tập huấn luyện. Ứng viên được chọn được giữ nguyên để đánh giá cuối cùng, không huấn luyện lại. Điểm xác thực của ứng viên được chọn phải khớp bảng kết quả xác thực.

| candidate_id | model | training_n | validation_n | validation_auuc | validation_qini | training_seconds | best_iteration | parameters | selected |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | S-Learner | 9786802 | 2097019 | 973.948 | -389.973 | 40.3498 | 199 | {"colsample_bytree": 0.8, "device_type": "cuda", "learning_rate": 0.05, "max_bin": 63, "max_depth": -1, "min_child_samples": 100, "n_estimators": 200, "n_jobs": -1, "num_leaves": 15, "random_state": 42, "reg_alpha": 0.0, "reg_lambda": 1.0, "subsample": 0.8, "subsample_freq": 1, "verbosity": -1} | True |
| 1 | S-Learner | 9786802 | 2097019 | 961.576 | -402.345 | 45.7339 | 210 | {"colsample_bytree": 0.8, "device_type": "cuda", "learning_rate": 0.05, "max_bin": 63, "max_depth": -1, "min_child_samples": 100, "n_estimators": 300, "n_jobs": -1, "num_leaves": 31, "random_state": 42, "reg_alpha": 0.0, "reg_lambda": 1.0, "subsample": 0.8, "subsample_freq": 1, "verbosity": -1} | False |
| fixed | Causal Forest | 200000 | 2097019 | 2069.58 | 705.656 |  |  | {"alpha": 0.05, "bootstrap": true, "ccp_alpha": 0.0, "control_name": 0, "criterion": "causal_mse", "groups_cnt": true, "groups_cnt_mode": "nodes", "groups_penalty": 0.5, "honesty_supported": false, "honesty_used": false, "max_depth": 10, "max_features": 1.0, "max_leaf_nodes": null, "max_samples": null, "min_group_samples": 50, "min_impurity_decrease": -Infinity, "min_samples_leaf": 100, "min_samples_split": 200, "min_weight_fraction_leaf": 0.0, "n_estimators": 100, "n_jobs": -1, "oob_score": false, "random_state": 42, "verbose": 0, "warm_start": false} | True |

## Kết quả validation

| model | auuc | qini | total_incremental_conversions | n |
| --- | --- | --- | --- | --- |
| Response model | 2592.06 | 1228.14 | 2727.84 | 2097019 |
| X-Learner | 2511.43 | 1147.51 | 2727.84 | 2097019 |
| T-Learner | 2183.41 | 819.491 | 2727.84 | 2097019 |
| Causal Forest | 2069.58 | 705.656 | 2727.84 | 2097019 |
| S-Learner | 973.948 | -389.973 | 2727.84 | 2097019 |

## Kết quả test

| model | auuc | qini | total_incremental_conversions | n |
| --- | --- | --- | --- | --- |
| Response model | 2289.11 | 1093.62 | 2390.98 | 2095771 |
| X-Learner | 2267.43 | 1071.94 | 2390.98 | 2095771 |
| T-Learner | 2121.32 | 925.829 | 2390.98 | 2095771 |
| Causal Forest | 1774.36 | 578.865 | 2390.98 | 2095771 |
| S-Learner | 909.012 | -286.479 | 2390.98 | 2095771 |

## Kết quả theo tỷ lệ tiếp cận

| model | target_rate | selected_n | incremental_conversions | policy_value_per_customer | ci_low_95 | ci_high_95 |
| --- | --- | --- | --- | --- | --- | --- |
| Response model | 0.05 | 104789 | 1988.63 | 0.000948876 |  |  |
| Response model | 0.1 | 209578 | 2096.47 | 0.00100033 | 1742.67 | 2429.82 |
| Response model | 0.2 | 419155 | 2289.8 | 0.00109258 | 1908.16 | 2627.31 |
| Response model | 0.3 | 628732 | 2335.29 | 0.00111429 |  |  |
| Response model | 0.4 | 838309 | 2352.16 | 0.00112233 |  |  |
| Response model | 0.5 | 1.04789e+06 | 2364.31 | 0.00112814 | 2007.59 | 2718.9 |
| Response model | 0.75 | 1.57183e+06 | 2385.1 | 0.00113805 |  |  |
| Response model | 1 | 2.09577e+06 | 2390.98 | 0.00114086 |  |  |
| S-Learner | 0.05 | 104789 | 617.647 | 0.000294711 |  |  |
| S-Learner | 0.1 | 209578 | 721.176 | 0.00034411 | 536.069 | 899.608 |
| S-Learner | 0.2 | 419155 | 864.706 | 0.000412596 | 648.559 | 1052.17 |
| S-Learner | 0.3 | 628732 | 918.824 | 0.000438418 |  |  |
| S-Learner | 0.4 | 838309 | 930.98 | 0.000444219 |  |  |
| S-Learner | 0.5 | 1.04789e+06 | 943.137 | 0.000450019 | 720.765 | 1141.73 |
| S-Learner | 0.75 | 1.57183e+06 | 964.314 | 0.000460124 |  |  |
| S-Learner | 1 | 2.09577e+06 | 2390.98 | 0.00114086 |  |  |
| T-Learner | 0.05 | 104789 | 1847.84 | 0.000881701 |  |  |
| T-Learner | 0.1 | 209578 | 1945.1 | 0.000928106 | 1641.13 | 2245.5 |
| T-Learner | 0.2 | 419155 | 2135.69 | 0.00101905 | 1802.32 | 2445.55 |
| T-Learner | 0.3 | 628732 | 2168.24 | 0.00103458 |  |  |
| T-Learner | 0.4 | 838309 | 2178.82 | 0.00103963 |  |  |
| T-Learner | 0.5 | 1.04789e+06 | 2178.43 | 0.00103944 | 1852.81 | 2505.95 |
| T-Learner | 0.75 | 1.57183e+06 | 2196.08 | 0.00104786 |  |  |
| T-Learner | 1 | 2.09577e+06 | 2390.98 | 0.00114086 |  |  |
| X-Learner | 0.05 | 104789 | 1900.39 | 0.000906775 |  |  |
| X-Learner | 0.1 | 209578 | 2097.25 | 0.00100071 | 1745.44 | 2424.41 |
| X-Learner | 0.2 | 419155 | 2233.73 | 0.00106583 | 1888.99 | 2575.77 |
| X-Learner | 0.3 | 628732 | 2320.78 | 0.00110737 |  |  |
| X-Learner | 0.4 | 838309 | 2327.84 | 0.00111073 |  |  |
| X-Learner | 0.5 | 1.04789e+06 | 2318.82 | 0.00110643 | 1965.75 | 2674.53 |
| X-Learner | 0.75 | 1.57183e+06 | 2349.41 | 0.00112103 |  |  |
| X-Learner | 1 | 2.09577e+06 | 2390.98 | 0.00114086 |  |  |
| Causal Forest | 0.05 | 104789 | 1294.12 | 0.00061749 |  |  |
| Causal Forest | 0.1 | 209578 | 1563.14 | 0.000745853 | 1284.59 | 1847.27 |
| Causal Forest | 0.2 | 419155 | 1659.22 | 0.000791697 | 1365.82 | 1977.34 |
| Causal Forest | 0.3 | 628732 | 1705.88 | 0.000813964 |  |  |
| Causal Forest | 0.4 | 838309 | 1746.67 | 0.000833424 |  |  |
| Causal Forest | 0.5 | 1.04789e+06 | 1786.27 | 0.000852323 | 1454.82 | 2097.26 |
| Causal Forest | 0.75 | 1.57183e+06 | 1944.31 | 0.000927732 |  |  |
| Causal Forest | 1 | 2.09577e+06 | 2390.98 | 0.00114086 |  |  |
| Random targeting | 0.05 | 104789 | 114.81 | 5.4782e-05 |  |  |
| Random targeting | 0.1 | 209578 | 227.961 | 0.000108772 |  |  |
| Random targeting | 0.2 | 419155 | 472.575 | 0.00022549 |  |  |
| Random targeting | 0.3 | 628732 | 701.307 | 0.00033463 |  |  |
| Random targeting | 0.4 | 838309 | 955.124 | 0.000455739 |  |  |
| Random targeting | 0.5 | 1.04789e+06 | 1196.46 | 0.000570891 |  |  |
| Random targeting | 0.75 | 1.57183e+06 | 1787.1 | 0.000852716 |  |  |
| Random targeting | 1 | 2.09577e+06 | 2390.98 | 0.00114086 |  |  |

## Khoảng tin cậy 95% của chính sách cố định

Khoảng tin cậy được tính bằng lấy mẫu lặp lại phân tầng theo nhóm treatment trên tập kiểm tra, có điều kiện theo thứ hạng điểm của từng mô hình.

| model | target_rate | selected_n | incremental_conversions | ci_low_95 | ci_high_95 | bootstrap_std | bootstrap_repeats | n_treated | n_control | conversions_treated | conversions_control |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Response model | 0.1 | 209578 | 2096.47 | 1742.67 | 2429.82 | 176.051 | 1000 | 180848 | 28730 | 4927 | 555 |
| Response model | 0.2 | 419155 | 2289.8 | 1908.16 | 2627.31 | 182.509 | 1000 | 359420 | 59735 | 5267 | 586 |
| Response model | 0.5 | 1047886 | 2364.31 | 2007.59 | 2718.9 | 180.381 | 1000 | 892349 | 155537 | 5472 | 611 |
| S-Learner | 0.1 | 209578 | 721.176 | 536.069 | 899.608 | 92.485 | 1000 | 179732 | 29846 | 1514 | 159 |
| S-Learner | 0.2 | 419155 | 864.706 | 648.559 | 1052.17 | 105.066 | 1000 | 357881 | 61274 | 1755 | 180 |
| S-Learner | 0.5 | 1047886 | 943.137 | 720.765 | 1141.73 | 109.259 | 1000 | 890946 | 156940 | 1952 | 203 |
| T-Learner | 0.1 | 209578 | 1945.1 | 1641.13 | 2245.5 | 157.557 | 1000 | 181292 | 28286 | 4141 | 439 |
| T-Learner | 0.2 | 419155 | 2135.69 | 1802.32 | 2445.55 | 168.647 | 1000 | 359566 | 59589 | 4473 | 469 |
| T-Learner | 0.5 | 1047886 | 2178.43 | 1852.81 | 2505.95 | 170.201 | 1000 | 892476 | 155410 | 4668 | 497 |
| X-Learner | 0.1 | 209578 | 2097.25 | 1745.44 | 2424.41 | 171.071 | 1000 | 180752 | 28826 | 4667 | 509 |
| X-Learner | 0.2 | 419155 | 2233.73 | 1888.99 | 2575.77 | 177.685 | 1000 | 359714 | 59441 | 4970 | 542 |
| X-Learner | 0.5 | 1047886 | 2318.82 | 1965.75 | 2674.53 | 187.523 | 1000 | 893016 | 154870 | 5286 | 585 |
| Causal Forest | 0.1 | 209578 | 1563.14 | 1284.59 | 1847.27 | 146.67 | 1000 | 179241 | 30337 | 3516 | 386 |
| Causal Forest | 0.2 | 419155 | 1659.22 | 1365.82 | 1977.34 | 156.255 | 1000 | 357275 | 61880 | 3881 | 436 |
| Causal Forest | 0.5 | 1047886 | 1786.27 | 1454.82 | 2097.26 | 161.186 | 1000 | 891146 | 156740 | 4397 | 508 |

Không suy luận mô hình nào vượt trội về mặt thống kê chỉ từ việc các khoảng tin cậy riêng lẻ chồng lấp hoặc không chồng lấp; cần khoảng tin cậy cho chênh lệch ghép cặp hoặc kiểm định trực tiếp.

## Kiểm tra cân bằng lựa chọn

Bảng đầy đủ được lưu tại `outputs_full_consistent_v2/tables/selection_balance_check.csv`.

| model | group_type | group | target_rate | decile | n | n_treated | n_control | treatment_rate | abs_deviation_from_0_85 | within_0_01 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Response model | top_k | top_10% | 0.1 |  | 209578 | 180848 | 28730 | 0.862915 | 0.012915 | False |
| Response model | top_k | top_20% | 0.2 |  | 419155 | 359420 | 59735 | 0.857487 | 0.00748709 | True |
| Response model | top_k | top_50% | 0.5 |  | 1047886 | 892349 | 155537 | 0.851571 | 0.00157069 | True |
| S-Learner | top_k | top_10% | 0.1 |  | 209578 | 179732 | 29846 | 0.85759 | 0.00759001 | True |
| S-Learner | top_k | top_20% | 0.2 |  | 419155 | 357881 | 61274 | 0.853815 | 0.00381541 | True |
| S-Learner | top_k | top_50% | 0.5 |  | 1047886 | 890946 | 156940 | 0.850232 | 0.0002318 | True |
| T-Learner | top_k | top_10% | 0.1 |  | 209578 | 181292 | 28286 | 0.865034 | 0.0150335 | False |
| T-Learner | top_k | top_20% | 0.2 |  | 419155 | 359566 | 59589 | 0.857835 | 0.00783541 | True |
| T-Learner | top_k | top_50% | 0.5 |  | 1047886 | 892476 | 155410 | 0.851692 | 0.00169188 | True |
| X-Learner | top_k | top_10% | 0.1 |  | 209578 | 180752 | 28826 | 0.862457 | 0.0124569 | False |
| X-Learner | top_k | top_20% | 0.2 |  | 419155 | 359714 | 59441 | 0.858188 | 0.0081885 | True |
| X-Learner | top_k | top_50% | 0.5 |  | 1047886 | 893016 | 154870 | 0.852207 | 0.00220721 | True |
| Causal Forest | top_k | top_10% | 0.1 |  | 209578 | 179241 | 30337 | 0.855247 | 0.00524721 | True |
| Causal Forest | top_k | top_20% | 0.2 |  | 419155 | 357275 | 61880 | 0.85237 | 0.00236965 | True |
| Causal Forest | top_k | top_50% | 0.5 |  | 1047886 | 891146 | 156740 | 0.850423 | 0.00042266 | True |
| Response model | decile | decile_1 |  | 1 | 209578 | 180848 | 28730 | 0.862915 | 0.012915 | False |
| Response model | decile | decile_2 |  | 2 | 209577 | 178572 | 31005 | 0.852059 | 0.00205915 | True |
| Response model | decile | decile_3 |  | 3 | 209577 | 177620 | 31957 | 0.847517 | 0.00248334 | True |
| Response model | decile | decile_4 |  | 4 | 209577 | 177412 | 32165 | 0.846524 | 0.00347581 | True |
| Response model | decile | decile_5 |  | 5 | 209577 | 177897 | 31680 | 0.848838 | 0.00116163 | True |
| Response model | decile | decile_6 |  | 6 | 209577 | 177730 | 31847 | 0.848042 | 0.00195847 | True |
| Response model | decile | decile_7 |  | 7 | 209577 | 177975 | 31602 | 0.849211 | 0.000789447 | True |
| Response model | decile | decile_8 |  | 8 | 209577 | 178179 | 31398 | 0.850184 | 0.000183942 | True |
| Response model | decile | decile_9 |  | 9 | 209577 | 177956 | 31621 | 0.84912 | 0.000880106 | True |
| Response model | decile | decile_10 |  | 10 | 209577 | 177774 | 31803 | 0.848251 | 0.00174852 | True |
| S-Learner | decile | decile_1 |  | 1 | 209578 | 179732 | 29846 | 0.85759 | 0.00759001 | True |
| S-Learner | decile | decile_2 |  | 2 | 209577 | 178149 | 31428 | 0.850041 | 4.07965e-05 | True |
| S-Learner | decile | decile_3 |  | 3 | 209577 | 177505 | 32072 | 0.846968 | 0.00303206 | True |
| S-Learner | decile | decile_4 |  | 4 | 209577 | 177631 | 31946 | 0.847569 | 0.00243085 | True |
| S-Learner | decile | decile_5 |  | 5 | 209577 | 177929 | 31648 | 0.848991 | 0.00100894 | True |
| S-Learner | decile | decile_6 |  | 6 | 209577 | 177927 | 31650 | 0.848982 | 0.00101848 | True |
| S-Learner | decile | decile_7 |  | 7 | 209577 | 177988 | 31589 | 0.849273 | 0.000727418 | True |
| S-Learner | decile | decile_8 |  | 8 | 209577 | 178135 | 31442 | 0.849974 | 2.60048e-05 | True |
| S-Learner | decile | decile_9 |  | 9 | 209577 | 177719 | 31858 | 0.847989 | 0.00201096 | True |
| S-Learner | decile | decile_10 |  | 10 | 209577 | 179248 | 30329 | 0.855285 | 0.00528469 | True |
| T-Learner | decile | decile_1 |  | 1 | 209578 | 181292 | 28286 | 0.865034 | 0.0150335 | False |
| T-Learner | decile | decile_2 |  | 2 | 209577 | 178274 | 31303 | 0.850637 | 0.000637236 | True |
| T-Learner | decile | decile_3 |  | 3 | 209577 | 177534 | 32043 | 0.847106 | 0.00289369 | True |
| T-Learner | decile | decile_4 |  | 4 | 209577 | 177739 | 31838 | 0.848084 | 0.00191553 | True |
| T-Learner | decile | decile_5 |  | 5 | 209577 | 177637 | 31940 | 0.847598 | 0.00240222 | True |
| T-Learner | decile | decile_6 |  | 6 | 209577 | 177914 | 31663 | 0.848919 | 0.00108051 | True |
| T-Learner | decile | decile_7 |  | 7 | 209577 | 177760 | 31817 | 0.848185 | 0.00181532 | True |
| T-Learner | decile | decile_8 |  | 8 | 209577 | 177929 | 31648 | 0.848991 | 0.00100894 | True |
| T-Learner | decile | decile_9 |  | 9 | 209577 | 178029 | 31548 | 0.849468 | 0.000531785 | True |
| T-Learner | decile | decile_10 |  | 10 | 209577 | 177855 | 31722 | 0.848638 | 0.00136203 | True |
| X-Learner | decile | decile_1 |  | 1 | 209578 | 180752 | 28826 | 0.862457 | 0.0124569 | False |
| X-Learner | decile | decile_2 |  | 2 | 209577 | 178962 | 30615 | 0.85392 | 0.00392004 | True |
| X-Learner | decile | decile_3 |  | 3 | 209577 | 177946 | 31631 | 0.849072 | 0.000927821 | True |
| X-Learner | decile | decile_4 |  | 4 | 209577 | 177673 | 31904 | 0.84777 | 0.00223045 | True |
| X-Learner | decile | decile_5 |  | 5 | 209577 | 177683 | 31894 | 0.847817 | 0.00218273 | True |
| X-Learner | decile | decile_6 |  | 6 | 209577 | 177937 | 31640 | 0.849029 | 0.000970765 | True |
| X-Learner | decile | decile_7 |  | 7 | 209577 | 177809 | 31768 | 0.848418 | 0.00158152 | True |
| X-Learner | decile | decile_8 |  | 8 | 209577 | 177705 | 31872 | 0.847922 | 0.00207776 | True |
| X-Learner | decile | decile_9 |  | 9 | 209577 | 177883 | 31694 | 0.848772 | 0.00122843 | True |
| X-Learner | decile | decile_10 |  | 10 | 209577 | 177613 | 31964 | 0.847483 | 0.00251674 | True |
| Causal Forest | decile | decile_1 |  | 1 | 209578 | 179241 | 30337 | 0.855247 | 0.00524721 | True |
| Causal Forest | decile | decile_2 |  | 2 | 209577 | 178034 | 31543 | 0.849492 | 0.000507928 | True |
| Causal Forest | decile | decile_3 |  | 3 | 209577 | 177967 | 31610 | 0.849172 | 0.000827619 | True |
| Causal Forest | decile | decile_4 |  | 4 | 209577 | 178005 | 31572 | 0.849354 | 0.000646302 | True |
| Causal Forest | decile | decile_5 |  | 5 | 209577 | 177899 | 31678 | 0.848848 | 0.00115208 | True |
| Causal Forest | decile | decile_6 |  | 6 | 209577 | 177925 | 31652 | 0.848972 | 0.00102802 | True |
| Causal Forest | decile | decile_7 |  | 7 | 209577 | 178058 | 31519 | 0.849607 | 0.000393411 | True |
| Causal Forest | decile | decile_8 |  | 8 | 209577 | 177941 | 31636 | 0.849048 | 0.000951679 | True |
| Causal Forest | decile | decile_9 |  | 9 | 209577 | 177969 | 31608 | 0.849182 | 0.000818076 | True |
| Causal Forest | decile | decile_10 |  | 10 | 209577 | 178924 | 30653 | 0.853739 | 0.00373872 | True |

## Kết luận theo chỉ số

- AUUC cao nhất: **Response model**.
- Qini cao nhất: **Response model**.

## Nhận xét dựa trên kết quả thực tế

1. Mô hình có AUUC cao nhất trên test là Response model.
2. Mô hình có Qini cao nhất trên test là Response model.
3. Tại tỷ lệ tiếp cận 10%, X-Learner đạt ước lượng 2,097.3 chuyển đổi tăng thêm trên tập đánh giá.
4. Tại tỷ lệ tiếp cận 20%, Response model đạt ước lượng 2,289.8 chuyển đổi tăng thêm trên tập đánh giá.
5. Tại tỷ lệ tiếp cận 50%, Response model đạt ước lượng 2,364.3 chuyển đổi tăng thêm trên tập đánh giá.

Không diễn giải ý nghĩa nghiệp vụ của f0–f11 và không xem feature importance là bằng chứng nhân quả.

## Cảnh báo và giới hạn

- Có 6 nhóm lớn lệch treatment rate quá 0.01 so với 0.85; xem selection_balance_check.csv.
- CausalML 0.17.0: phiên bản đang dùng không hỗ trợ ước lượng trung thực (honesty).
- Rừng nhân quả dùng n_jobs=-1; huấn luyện trên 200,000 quan sát.
- Rừng nhân quả sử dụng mẫu con phân tầng, khác quy mô huấn luyện của các mô hình hiệu ứng tăng thêm còn lại.

## Tệp đầu ra

- `outputs_full_consistent_v2/figures/incremental_conversions_by_target_rate.png`
- `outputs_full_consistent_v2/figures/qini_curves.png`
- `outputs_full_consistent_v2/figures/uplift_by_decile.png`
- `outputs_full_consistent_v2/figures/uplift_curves.png`
- `outputs_full_consistent_v2/models/causal_forest.joblib`
- `outputs_full_consistent_v2/models/response_model.joblib`
- `outputs_full_consistent_v2/models/s_learner.joblib`
- `outputs_full_consistent_v2/models/t_learner.joblib`
- `outputs_full_consistent_v2/models/x_learner.joblib`
- `outputs_full_consistent_v2/run_config.json`
- `outputs_full_consistent_v2/tables/decile_results.csv`
- `outputs_full_consistent_v2/tables/environment_versions.csv`
- `outputs_full_consistent_v2/tables/errors_or_warnings.csv`
- `outputs_full_consistent_v2/tables/feature_importance.csv`
- `outputs_full_consistent_v2/tables/model_parameters.csv`
- `outputs_full_consistent_v2/tables/random_targeting_results.csv`
- `outputs_full_consistent_v2/tables/selection_balance_check.csv`
- `outputs_full_consistent_v2/tables/split_summary.csv`
- `outputs_full_consistent_v2/tables/targeting_policy_ci.csv`
- `outputs_full_consistent_v2/tables/targeting_policy_results.csv`
- `outputs_full_consistent_v2/tables/test_metrics.csv`
- `outputs_full_consistent_v2/tables/training_log.csv`
- `outputs_full_consistent_v2/tables/training_sample_summary.csv`
- `outputs_full_consistent_v2/tables/uplift_score_summary.csv`
- `outputs_full_consistent_v2/tables/validation_metrics.csv`
- `outputs_full_consistent_v2/test_predictions.parquet`

## Chi tiết số quan sát và thời gian theo mô hình

Thời gian của S-Learner là thời gian học của ứng viên được chọn; thời gian học của từng ứng viên được ghi riêng trong bảng siêu tham số. Rừng nhân quả thực tế học trên 200,000 quan sát.

| model | start | end | training_seconds | cache_or_total_seconds | prediction_seconds | training_n | component_sample_and_conversion_counts | best_iterations | status | error |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| response_model | 2026-09-30T13:55:42.427296+00:00 | 2026-09-30T13:56:13.265431+00:00 | 30.8381 | 30.8745 | 8.80996 | 8317946 | {"treated_n": 8317946, "treated_conversions": 25560} | {"main": 150} | success |  |
| s_learner | 2026-09-30T13:52:49.784431+00:00 | 2026-09-30T13:53:30.134238+00:00 | 40.3498 | 0.032072 | 25.1902 | 9786802 | {"all_n": 9786802, "conversions": 28423} | {"main": 199} | success |  |
| t_learner | 2026-09-30T13:56:13.334599+00:00 | 2026-09-30T13:56:50.223814+00:00 | 36.8892 | 36.9689 | 14.9263 | 9786802 | {"treated_n": 8317946, "control_n": 1468856, "treated_conversions": 25560, "control_conversions": 2863} | {"mu1": 150, "mu0": 98} | success |  |
| x_learner | 2026-09-30T13:56:50.304501+00:00 | 2026-09-30T13:58:37.622122+00:00 | 107.318 | 107.389 | 11.3148 | 9786802 | {"treated_n": 8317946, "control_n": 1468856, "treated_conversions": 25560, "control_conversions": 2863} | {"mu1": 150, "mu0": 98, "tau1": 193, "tau0": 106} | success |  |
| causal_forest | 2026-09-30T13:58:37.694326+00:00 | 2026-09-30T14:01:37.576632+00:00 | 179.882 | 183.593 | 14.4779 | 200000 | {"all_n": 200000, "treated_n": 169983, "control_n": 30017, "conversions": 580, "treated_conversions": 522, "control_conversions": 58} | {"n_estimators": 100} | success |  |