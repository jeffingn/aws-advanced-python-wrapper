# Plugin Pipeline Performance Results

## Performance Tests

### Enhanced Failure Monitoring Performance with Different Failure Detection Configuration

| FailureDetectionGraceTime | FailureDetectionInterval | FailureDetectionCount | NetworkOutageDelayMillis | MinFailureDetectionTimeMillis | MaxFailureDetectionTimeMillis | AvgFailureDetectionTimeMillis |
|---------------------------|--------------------------|-----------------------|--------------------------|-------------------------------|-------------------------------|-------------------------------|
| 30000                     | 5000                     | 3                     | 5000                     | 41096                         | 41102                         | 41099                         |
| 30000                     | 5000                     | 3                     | 10000                    | 36095                         | 36100                         | 36098                         |
| 30000                     | 5000                     | 3                     | 15000                    | 31093                         | 31102                         | 31098                         |
| 30000                     | 5000                     | 3                     | 20000                    | 26099                         | 26101                         | 26100                         |
| 30000                     | 5000                     | 3                     | 25000                    | 21095                         | 21105                         | 21102                         |
| 30000                     | 5000                     | 3                     | 30000                    | 16094                         | 16103                         | 16099                         |
| 30000                     | 5000                     | 3                     | 35000                    | 15010                         | 15409                         | 15209                         |
| 30000                     | 5000                     | 3                     | 40000                    | 15116                         | 15126                         | 15121                         |
| 30000                     | 5000                     | 3                     | 50000                    | 15137                         | 15145                         | 15141                         |
| 30000                     | 5000                     | 3                     | 60000                    | 15060                         | 15170                         | 15105                         |
| 6000                      | 1000                     | 1                     | 1000                     | 6110                          | 7115                          | 6693                          |
| 6000                      | 1000                     | 1                     | 2000                     | 5111                          | 6114                          | 5493                          |
| 6000                      | 1000                     | 1                     | 3000                     | 4110                          | 5114                          | 4673                          |
| 6000                      | 1000                     | 1                     | 4000                     | 3109                          | 4115                          | 3492                          |
| 6000                      | 1000                     | 1                     | 5000                     | 2211                          | 3112                          | 2732                          |
| 6000                      | 1000                     | 1                     | 6000                     | 1110                          | 2114                          | 1512                          |
| 6000                      | 1000                     | 1                     | 7000                     | 1112                          | 1213                          | 1153                          |
| 6000                      | 1000                     | 1                     | 8000                     | 1116                          | 1219                          | 1139                          |
| 6000                      | 1000                     | 1                     | 9000                     | 1118                          | 1218                          | 1139                          |
| 6000                      | 1000                     | 1                     | 10000                    | 1120                          | 1222                          | 1141                          |


### Failover Performance with Different Enhanced Failure Monitoring Configuration

| FailureDetectionGraceTime | FailureDetectionInterval | FailureDetectionCount | NetworkOutageDelayMillis | MinFailureDetectionTimeMillis | MaxFailureDetectionTimeMillis | AvgFailureDetectionTimeMillis |
|---------------------------|--------------------------|-----------------------|--------------------------|-------------------------------|-------------------------------|-------------------------------|
| 30000                     | 5000                     | 3                     | 5000                     | 40663                         | 45565                         | 43600                         |
| 30000                     | 5000                     | 3                     | 10000                    | 35551                         | 40574                         | 37575                         |
| 30000                     | 5000                     | 3                     | 15000                    | 30534                         | 35552                         | 33540                         |
| 30000                     | 5000                     | 3                     | 20000                    | 25562                         | 30613                         | 27579                         |
| 30000                     | 5000                     | 3                     | 25000                    | 20623                         | 25557                         | 23571                         |
| 30000                     | 5000                     | 3                     | 30000                    | 15504                         | 20551                         | 17541                         |
| 30000                     | 5000                     | 3                     | 35000                    | 15474                         | 15641                         | 15573                         |
| 30000                     | 5000                     | 3                     | 40000                    | 15553                         | 15623                         | 15581                         |
| 30000                     | 5000                     | 3                     | 50000                    | 15546                         | 15650                         | 15578                         |
| 30000                     | 5000                     | 3                     | 60000                    | 15519                         | 15628                         | 15594                         |
| 6000                      | 1000                     | 1                     | 1000                     | 6555                          | 7531                          | 7116                          |
| 6000                      | 1000                     | 1                     | 2000                     | 5541                          | 6525                          | 5949                          |
| 6000                      | 1000                     | 1                     | 3000                     | 4518                          | 5540                          | 5115                          |
| 6000                      | 1000                     | 1                     | 4000                     | 3519                          | 4419                          | 3891                          |
| 6000                      | 1000                     | 1                     | 5000                     | 2544                          | 3507                          | 3108                          |
| 6000                      | 1000                     | 1                     | 6000                     | 1501                          | 2433                          | 1890                          |
| 6000                      | 1000                     | 1                     | 7000                     | 1418                          | 1513                          | 1491                          |
| 6000                      | 1000                     | 1                     | 8000                     | 1505                          | 1544                          | 1523                          |
| 6000                      | 1000                     | 1                     | 9000                     | 1534                          | 1656                          | 1586                          |
| 6000                      | 1000                     | 1                     | 10000                    | 1554                          | 1582                          | 1568                          |

