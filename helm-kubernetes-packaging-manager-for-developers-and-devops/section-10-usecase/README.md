helm create coupon-service-chart

helm dependency update coupon-service-chart

# after update files
cd coupon-service-chart
helm install couponservice .

# connect to mysql
# infos from values.yaml
pass: test1234 //should be in vault.
port: 32762

INSERT INTO `coupon` (`id`,`code`,`discount`,`exp_date`) VALUES (2,'UDEMY_SALE',1.000,'01.01.2022');

# check endpoint
# uses values.yaml for nodeport info
#
# service:
#    type: NodePort
#    port: 9091
#    targetPort: 9091
#    nodePort: 32761

curl http://localhost:32761/couponapi/coupons/UDEMY_SALE

# check probes, details in deployment.yaml
curl http://localhost:32761/actuator/health/readiness
curl http://localhost:32761/actuator/health/liveness
