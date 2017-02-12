
NAME=puppet-test-1
IMAGE=el7-systemd

run:
	docker inspect $(NAME) &>/dev/null || \
	docker run -d -t -e FACTER_cluster=jenkins-slave \
		--add-host puppet.int.xcalar.com:127.0.0.1 \
		--add-host graphite.int.xcalar.com:127.0.0.1 \
		--dns 10.10.2.136 \
		--dns 10.10.1.1 \
		--dns-search int.xcalar.com \
		--privileged \
		--name $(NAME) \
		-v /sys/fs/cgroup:/sys/fs/cgroup:ro \
		-v `pwd`:/etc/puppetlabs/code \
		-v /netstore:/netstore $(IMAGE)
	#docker run -d -t -e FACTER_cluster=jenkins-slave --add-host puppet.int.xcalar.com:127.0.0.1 --add-host graphite.int.xcalar.com:127.0.0.1 --dns 10.10.2.136 --dns 10.10.1.1 --dns-search int.xcalar.com --privileged --name $(NAME) -v /sys/fs/cgroup:/sys/fs/cgroup:ro -v `pwd`:/etc/puppetlabs/code -v /netstore:/netstore el7-systemd

#stuff:
#docker run -d -t -p 22 --dns 10.10.2.136 --dns 10.10.1.1 --dns-search int.xcalar.com --privileged --name $(NAME) -v /sys/fs/cgroup:/sys/fs/cgroup:ro -v `pwd`:/etc/puppetlabs/code el7-systemd
#ipaddress=`docker inspect --format='{{ (index (index .NetworkSettings.Ports "22/tcp") 0).HostPort }}' $(NAME)`; ssh -i id_rsa -oPort=$$ipaddress root@0.0.0.0

apply:
	docker exec -ti $(NAME) /opt/puppetlabs/bin/puppet apply --debug -t /etc/puppetlabs/code/environments/production/manifests/site.pp

shell:
	docker exec -ti $(NAME) bash -l

clean:
	docker rm -fv $(NAME) || true
