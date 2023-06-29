cd $HOME
wget -q -O $HOME/install.sh https://raw.githubusercontent.com/bloxapp/ssv/main/install.sh
chmod +x $HOME/install.sh
./install.sh
docker run -d --name=ssv_node_op_key -it 'bloxstaking/ssv-node:latest' \
/go/bin/ssvnode generate-operator-keys && docker logs ssv_node_op_key --follow \
&& docker stop ssv_node_op_key && docker rm ssv_node_op_key
wget -q -O beacon-prater.sh https://api.nodes.guru/beacon-prater.sh && chmod +x beacon-prater.sh && sudo /bin/bash beacon-prater.sh
export SSV_DB=$HOME/.ssv
mkdir -p $SSV_DB
yq n db.Path "$SSV_DB" | tee $SSV_DB/config.yaml \
&& yq w -i $SSV_DB/config.yaml eth2.Network "prater" \
&& yq w -i $SSV_DB/config.yaml eth2.BeaconNodeAddr "YOUR_HTTPS_BEACON_ETH2_ENDPOINT" \
&& yq w -i $SSV_DB/config.yaml eth1.ETH1Addr "YOUR_WSS_GOERLI_ETH_ENDPOINT" \
&& yq w -i $SSV_DB/config.yaml OperatorPrivateKey "YOUR_PRIVATE_KEY_FROM_2.1" \
&& yq w -i $SSV_DB/config.yaml MetricsAPIPort "15000"
export SSV_DB=$HOME/.ssv
mkdir -p $SSV_DB
yq n db.Path "$SSV_DB" | tee $SSV_DB/config.yaml \
&& yq w -i $SSV_DB/config.yaml eth2.Network "prater" \
&& yq w -i $SSV_DB/config.yaml eth2.BeaconNodeAddr "http://135.181.205.9:5052" \
&& yq w -i $SSV_DB/config.yaml eth1.ETH1Addr "wss://goerli.infura.io/ws/v3/32240b05234QW30b0c95992aa2ea9fd" \
&& yq w -i $SSV_DB/config.yaml OperatorPrivateKey "LS0tLS1CRUdJTiBSU0EgUFJJVkFURSBLRVktLS0tLQpNSUlFcEFJQkFBS0NBUUVBMGhsasdasR1FaWXFwSitCbHVYbnJ6WlltSTFSTldsNW9KMTlKeXB5MlE4ClFRS1hQVno5Q2VwYVljNUFLeW54VmSADQVhYSTU1L2xSVnZMNzNpNlVJenhuMGd5TFJwSzFSSG1lcXcKSGIwNnpqU1ZmeW92aWp5OG1ZNXJKOGhkaHVPT283UjltZm5zUmFiMzM2alVZckRpZVJBNFU3UVdGbzhaZ1U4TAo1MnhNVlM1clNvNWp1QTcvV2IwWDdQVUMwYTVwdURKMi9lYUNjUmtwTTEyaE5qemRrZ3NIRjdrWGV2dnNLaGQyCjZCSEx0L0FoT0JVVGNjc0wxd2d3anZhbGRuVlF6cEczdjFxeUJCbHVsTlRDeG9Bb0I3eEw1WHlYaGVhNFFEM3MKVFhmdXVLTDNaMjFHMDFaclVBNGhVZE9KOC9qS0RUcnlNTmQwR1FJREFRQUJBb0lCQUFsL3NuMnZWbk5qY3ZCKwpzL1dLUi81Ymw4QktnMzJLT1NSU2tIeWFlMkRDMEg1c21yakp1RExEQ0lHeFFjTFlRMWh3UGdRRWhjNkgyYTIrCndZcFB1Uzd4V0NTTmJqdmpqVm0xQUkxeWNxck4xYXJrekcrYnhaUGJGc000RWd3cThNLzBlMjNDQmxCNTYxSUYKY0FSTkkxeUd6d0FpcnUrRFZjbnBiTktEK3Rlc0xZYVhJYlpkMXB6L3ZZSnJ5c0RpUXhnZjFsZGZ3VGV3aDVnUAo2eW9mYTVzcGt5eURPckF0UDhsRmg1cmRpZHVXOXRaaHJpZGxQNkd6a1YxV1gvR1N6djd5MWVZSjVGVElRNXd6CkJ3b2NrUksxYmk0RFNIZWFhMUxwQ3VJczE5M1NWNnc5bUR0cW02cG5ZWGNxY2Zqemc1MTRmWUt0YWxVbVkvTXEKQWRVTm5HMENnWUVBLzEyeG4vTk9zc3cveE81SktFNmtrUFhJcG1ENjZNWHZKeEtOaGVWNHBLUm93YS9jWlVEYwpBQk95SkJvQVZKSlM1ZE5SMG1LVFpIY2FDMDdFYlg1WHNuYUVxQ2k0WDJScmxDdjQvMVNLcUVuTWs2SHd0Z2tECitlajBZWWpEZldjWk5QMitQbEZBRS9Kd1VVcUcyNERIUG4vY0Vob3JmaUdPVmdpWWhxbFN5Z01DZ1lFQTBwN3YKajVmaFlhWWF3WG5LOW1nMGJ3eGFMbVdGYVg2WDFvTElBQW56TkEvT0x0UVVodXJ0OVRWUUZheFNVUEFGQUxGeQpPTEJtTlpRRmJTNjNSQkRDMzVOVXQ4SENBQWR5S01ROHIxVlZWZWhrekNMamZXb0lsYThmTWpvdjVaNWxXL3Y1ClgvZE9xQ1MyMmJ0ekpyQUFPT2tSdjNUcmI3eVVzWktYY3B2ZXZMTUNnWUVBdERxK1dITnlRY0xoNXUxK2VKSTQKbDQ3QWxKeXh1WU9wTEZ4dHdlaVk0eXFlUE1VUHVlNnkxdU1Qd1djUWdKL2RIaE90RmQwNHhabEJxbVZuUVJnQQpjUll3dmpZdkdqUlhzUFh3QU5UOEg1WW1hclBLbmM5ekhQaUxNaU5DUmNsMEF2QnJTclF4azJKeVh3MUliTGRRCjZBMTVsdFVkaVNISndYTERvNWJ4dFYwQ2dZQm5RSU13WWNTYm1sS0RockY1R3B5WjAvUmh5bU1jMUhLYk5vSFYKZ3dzMitvaVpiRjZqeFlmaUxjQ1UzMlY3U0M4cnA5SFcrS01pb080SUdGNi9mM2dKSXJEbFpKYzJiSEFLakNregp5eUdLZStMdU1DeFZ6eVZtTy9PUEsvZFBHZGVXd0hBQUc3enpzUXZ5c1hKQ0JRWVc0Q3RUTUg0NnlMZ2NWVURwCmJvQkRBUUtCZ1FEQlgrTnhWTURFMU5hWWRyUStZYm9ZREV0K0VoanVSbGczV1lXdkNDbk9hbktLV2cvMzVNY3MKNlQvc3Q2VVBKbDlSZmRwN2ZWbXQyek81QUEvUHB4Qi95K2ZIaTlQdTVJZFRRcWxaNFBMcDVFVzRwSksyZ0h3Rgo4ekllTU1WYXVpOXlzb09helpQditmMy9YVHM3MW0zSTBEY1ZveU1saEthYkw2R3RNRzBHbXc9PQotLS0tLUVORCBSU0EgUFJJVkFURSBLRVktLS0tLQo" \
&& yq w -i $SSV_DB/config.yaml MetricsAPIPort "15000"
