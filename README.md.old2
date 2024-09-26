
IaC HW3
........ ....... . ....... ............. ................. .... .. Terraform.
..........
........ ............... ........ . Yandex.Cloud

............ ...... .......... ......... ... .. ........ ......... .......... - ....... . .. ....... ............... ..........

.., ... ... ... .... .. ....... ...... ........ ......, .. .. ..... ......... ..... . ......... ........ Yandex.Cloud.

......., .......... ...., ... .......... ....... .otus. . ....... .., ........ . ... .. ...... ... .... ....... - ....... ... .stage.. . ......../....... ID .......... .........

........., ....... .. .. ...... .... ......., ...... ......... ........ ...:

......... ....... .......

... ....... ...... ........... .......... .. ....... ....... GoLang.

.......... .. ......... .. ......... .. ..... ..... ...: https://golang.org/doc/install

............. ....... ......... .......
.... ......

....... ....., ........... ......... .. ....... ....... ... ...... .........-...........

......... ........:

    ....... IP Load balancer-. . state-. ..........
    ........... ............ .. ssh . ..... .. ........... .....
    ........... ........... . .... ...... (... ....... ... ...........)

........ ......
.......... . ............ ............

... .......... ............ ..... .............. Go-.......... - Terratest.

. ..... ...... ..... ........ .. .......... ............ .......... ............ ..... Go.

. ....... Terratest .. ...... .......... ........ ....., ......... ......... .........., ......... ........... ..... . ....... ........ ......

... ... ..... ....... ... ...... ..... ....... (. ..... .........., . ....... ....)

go test

..... ......... .......... . Terratest .. ....... ..... .. ..... https://terratest.gruntwork.io/docs.

........ ... ..... .......... ...: https://github.com/gruntwork-io/terratest.
..... .....

....... ........ ..... ..... . ........... . ........ . ...

. git switch -c ansible
Switched to a new branch 'ansible'

....., ...... ........ .... ...... . output-........... ...........

....... ....... ..... .......... load_balancer_public_ip .. .........:

output "load_balancer_public_ip" {
  description = "Public IP address of load balancer"
  value = tolist(tolist(yandex_lb_network_load_balancer.wp_lb.listener).0.external_address_spec).0.address
}

. ..... .... ..... .......... ..... ......... ... (. ... IP ..... ......):

load_balancer_public_ip = "84.201.165.8"

. ....... ... .... .......... ... ........... IP ..... .. ........... .....

output "vm_linux_public_ip_address" {
  description = "Virtual machine IP"
  value = yandex_compute_instance.wp-app-1.network_interface[0].nat_ip_address
}

. ..... .... output.tf ...... ......... ...:

output "load_balancer_public_ip" {
  description = "Public IP address of load balancer"
  value = tolist(tolist(yandex_lb_network_load_balancer.wp_lb.listener).0.external_address_spec).0.address
}

output "database_host_fqdn" {
  description = "DB hostname"
  value = local.dbhosts
}

output "vm_linux_public_ip_address" {
  description = "Virtual machine IP"
  value = yandex_compute_instance.wp-app-1.network_interface[0].nat_ip_address
}

.gitignore

...... ... .. ......... . ......... ...... ........ .... .gitignore ...... . ............ ... ..... Go

# Created by https://www.toptal.com/developers/gitignore/api/go
# Edit at https://www.toptal.com/developers/gitignore?templates=go

### Go ###
# Binaries for programs and plugins
*.exe
*.exe~
*.dll
*.so
*.dylib

# Test binary, built with `go test -c`
*.test

# Output of the go coverage tool, specifically when used with LiteIDE
*.out

# Dependency directories (remove the comment below to include it)
# vendor/

### Go Patch ###
vendor
Godeps

# End of https://www.toptal.com/developers/gitignore/api/go

Terratest
............. .......

...., ...... ..... ............... ... .......

........ . ........ terraform .......... test.

. test ........ .... end2end_test.go .. ......... ..........:

package test

import (
    "testing"
)

func TestEndToEndDeploymentScenario(t *testing.T) {
}

....... ...... .. ........ .... ....., . .... ........ ....... ............. ...... Go

. go mod init test
go: creating new go.mod: module test
go: to add module requirements and sums:
    go mod tidy

... .. ...... ........ - . ........ ........ .... go.mod

. head go.mod
module test

go 1.17

...... . ... .......... ......., . .......... ... ..... ......... ...... .. .........., ....... ..... .......... ... ...... .......
........ ..............

... ... .... .. ........ ..... Terratest ......... . ....... ........... ......., .. .. ...... . ...., ... ......... end2end_test.go ............ ... ..... .........:

package test

import (
    "fmt"
    "flag"
    "testing"
    "time"

    "github.com/gruntwork-io/terratest/modules/terraform"
)

var folder = flag.String("folder", "", "Folder ID in Yandex.Cloud")

func TestEndToEndDeploymentScenario(t *testing.T) {

    terraformOptions := &terraform.Options{
	    TerraformDir: "../",

	    Vars: map[string]interface{}{
	    "yc_folder":    *folder,
	    },
    }

    defer terraform.Destroy(t, terraformOptions)

    terraform.InitAndApply(t, terraformOptions)

    fmt.Println("Finish infra.....")

    time.Sleep(30 * time.Second)

    fmt.Println("Destroy infra.....")
}

........ ........, ... ...... ... ........

var folder = flag.String("folder", "", "Folder ID in Yandex.Cloud")

... .. .......... .......... folder, ....... ..... ......... ........ ID ........, ... ..... ........... ........ ... .. ......., .. ...... ... ...... . ........ stage Yandex.Cloud. ... ... ............ ID ........ - ...... ...., .. ..... .......... ... . ........ ...../..... . ....... ....... .......

    terraformOptions := &terraform.Options{
	    TerraformDir: "../",

	    Vars: map[string]interface{}{
	    "yc_folder":    *folder,
	    },
    }

... .. ........ Terratest, ... ......... ......... .......... TerraformDir: "../" . ...... ........ input-.......... .......... yc_folder ...... ID ........, ....... ..... ........ ... ....... ...... . ......... . .......... folder.

    defer terraform.Destroy(t, terraformOptions)

... ...... ......... defer .. ........ ....... terraform.Destroy ..... .......... ..... ....... .... ..... .... ...... ........ ....... TestEndToEndDeploymentScenario.

    terraform.InitAndApply(t, terraformOptions)

... ........... ....... init . apply ... .......... ...........

    fmt.Println("Finish infra.....")

    time.Sleep(30 * time.Second)

    fmt.Println("Destroy infra.....")

..... .. ....... . ....... .............. .......... ............ ......... . ...... ......... ....., . ....... ....... .. ...... ....... . ...-....... Yandex.Cloud . ........., ... ....... ............. ........

import (
    "fmt"
    "flag"
    "testing"
    "time"

    "github.com/gruntwork-io/terratest/modules/terraform"
)

... .., . ...... import .. ........ ... ........... ... .........., ....... ............... terratest "github.com/gruntwork-io/terratest/modules/terraform"

...... ... ......... ...., ........ ......., ........... ........... ...........

go mod vendor

...... .. ..... ......... ...., ...... . ........ ..... ..... .. ....... .. ...... ........... (...... .. ......... ... ....... ..... ...... ...: https://terratest.gruntwork.io/docs/testing-best-practices/timeouts-and-logging/) . ......... ID ........ stage (. ... ID ..... .........., .......)

go test -v ./ -timeout 30m -folder 'b1guvpc4h7oehnd9pabt'

........ ........, ... ....... . .... ..... .... ......... . ........ ........ (... ... ...... ... ...... ....... terraform apply), . . ..... - ........... .. .. ............

. ..... .. ........ ........., ... ............ ...... ... .......

...
--- PASS: TestEndToEndDeploymentScenario (515.93s)
PASS
ok  	test	516.210s

.......... ........ ............

... .. ....., ...... ........ . ........ .............. ...... ......., .. ..... ..... 10 ...... .. ....... ..... ......... ....... ....... ........ ........ ... .......

.........., ... .... .. ..... .......... ....., ...... ........ ..... ........ .......... ..... ........

..... ..... .... .. ........, .... .. ... .. ....... ...... ... ......... ..... . ...... ......, .... . ........ ....... .......... .......... ...........

. ......., Terratest ......... ... ... ........

........ ..... end2end_test.go ......... ...:

package test

import (
    "fmt"
    "flag"
    "testing"

    "github.com/gruntwork-io/terratest/modules/terraform"
    test_structure "github.com/gruntwork-io/terratest/modules/test-structure"
)

var folder = flag.String("folder", "", "Folder ID in Yandex.Cloud")

func TestEndToEndDeploymentScenario(t *testing.T) {
    fixtureFolder := "../"

    test_structure.RunTestStage(t, "setup", func() {
	terraformOptions := &terraform.Options{
	    TerraformDir: fixtureFolder,

	    Vars: map[string]interface{}{
	    "yc_folder":    *folder,
	    },
        }

	test_structure.SaveTerraformOptions(t, fixtureFolder, terraformOptions)

	terraform.InitAndApply(t, terraformOptions)
    })

    test_structure.RunTestStage(t, "validate", func() {
        fmt.Println("Run some tests...")

    })

    test_structure.RunTestStage(t, "teardown", func() {
	terraformOptions := test_structure.LoadTerraformOptions(t, fixtureFolder)
	terraform.Destroy(t, terraformOptions)
    })
}

... .......... - ...... ...... ...... ........... ........ ............ (........ .............., .......... ....., ........ ..............) .. .......... . ........... ....... test_structure.RunTestStage. ..... .. .......... ..... ....... ... ...... (setup/validate.teardown) . ............. ......., . ....... . .......... ........... .........

...., ...... ......... .... imports - ......... ...... test_structure "github.com/gruntwork-io/terratest/modules/test-structure"

... ... ........ .......

go mod vendor

. ........ ....

go test -v ./ -timeout 30m -folder 'b1guvpc4h7oehnd9pabt'

. ... ..... ... .. .......... ........ . ........ .............., .. ........ ........ .. ......... ......, ....... ......... . ....:

...
TestEndToEndDeploymentScenario 2021-09-10T22:21:01+03:00 test_structure.go:26: The 'SKIP_setup' environment variable is not set, so executing stage 'setup'.
...
TestEndToEndDeploymentScenario 2021-09-10T22:27:50+03:00 test_structure.go:26: The 'SKIP_validate' environment variable is not set, so executing stage 'validate'.
...
TestEndToEndDeploymentScenario 2021-09-10T22:27:50+03:00 test_structure.go:26: The 'SKIP_teardown' environment variable is not set, so executing stage 'teardown'.

....., ........... ......... .......... ........., ... ....... ....... .. ........ .SKIP_. . ..... ....... . ...... ... ...... .... .......... .. ..... ......... ..., ..... .. ........... .. ... .... ......, ... ....

........ ... .. .... ........ .. ....., ..... ... ......... ....... ........... ...... ...... setup (........ ..............).

... ..... .. ......... ... .......... .........:

export SKIP_validate=true

export SKIP_teardown=true

. ..... ........ ....:

go test -v ./ -timeout 30m -folder 'b1guvpc4h7oehnd9pabt'

.. .... ... .......... ...... ...... ........ ............... ...... ............... ............ . ........ .............. .. ............

.............., . .... ..... .......:

...
TestEndToEndDeploymentScenario 2021-09-10T22:34:31+03:00 test_structure.go:26: The 'SKIP_setup' environment variable is not set, so executing stage 'setup'.
...
TestEndToEndDeploymentScenario 2021-09-10T22:40:58+03:00 test_structure.go:29: The 'SKIP_validate' environment variable is set, so skipping stage 'validate'.
...
TestEndToEndDeploymentScenario 2021-09-10T22:40:58+03:00 test_structure.go:29: The 'SKIP_teardown' environment variable is set, so skipping stage 'teardown'.

., .... ... ......... ........., .. ..... ..... ........... ...... ...... validate, .. .. ....... .........:

export SKIP_setup=true

unset SKIP_validate

. ........, .......... .. .., ... ... .....:

. go test -v ./ -timeout 30m -folder 'b1guvpc4h7oehnd9pabt'
=== RUN   TestEndToEndDeploymentScenario
TestEndToEndDeploymentScenario 2021-09-10T22:56:09+03:00 test_structure.go:29: The 'SKIP_setup' environment variable is set, so skipping stage 'setup'.
TestEndToEndDeploymentScenario 2021-09-10T22:56:09+03:00 test_structure.go:26: The 'SKIP_validate' environment variable is not set, so executing stage 'validate'.
Run some tests...
TestEndToEndDeploymentScenario 2021-09-10T22:56:09+03:00 test_structure.go:29: The 'SKIP_teardown' environment variable is set, so skipping stage 'teardown'.
--- PASS: TestEndToEndDeploymentScenario (0.00s)
PASS
ok  	test	0.406s

.., .. ........ .., ... .......

. .......-...
............. ..... .....

...., ........ .......... . ..... ......

    ......... ........:

        ....... IP Load balancer-. . state-. ..........
        ........... ............ .. ssh . ..... .. ........... .....
        ........... ........... . .... ...... (... ....... ... ...........)

....... ... ........

... ........ ....... IP .............. ....... . ...... validate ..... .......

fmt.Println("Run some tests...")

......... ....

        terraformOptions := test_structure.LoadTerraformOptions(t, fixtureFolder)

        // test load balancer ip existing
        loadbalancerIPAddress := terraform.Output(t, terraformOptions, "load_balancer_public_ip")

        if loadbalancerIPAddress == "" {
	    t.Fatal("Cannot retrieve the public IP address value for the load balancer.")
	}

... .. ........ .......... ... terratest-. ...... ...... ...........

..... ......... .......... loadbalancerIPAddress ........ IP, ......., ... .. ......., ...... .... . output-.......... ..........

. ..... ........., ... ........ loadbalancerIPAddress .. ........

........ ... end2end_test.go ...... ......... ...:

package test

import (
    "fmt"
    "flag"
    "testing"

    "github.com/gruntwork-io/terratest/modules/terraform"
    test_structure "github.com/gruntwork-io/terratest/modules/test-structure"
)

var folder = flag.String("folder", "", "Folder ID in Yandex.Cloud")

func TestEndToEndDeploymentScenario(t *testing.T) {
    fixtureFolder := "../"

    test_structure.RunTestStage(t, "setup", func() {
	terraformOptions := &terraform.Options{
	    TerraformDir: fixtureFolder,

	    Vars: map[string]interface{}{
	    "yc_folder":    *folder,
	    },
        }

	test_structure.SaveTerraformOptions(t, fixtureFolder, terraformOptions)

	terraform.InitAndApply(t, terraformOptions)
    })

    test_structure.RunTestStage(t, "validate", func() {
        fmt.Println("Run some tests...")

        terraformOptions := test_structure.LoadTerraformOptions(t, fixtureFolder)

            // test load balancer ip existing
        loadbalancerIPAddress := terraform.Output(t, terraformOptions, "load_balancer_public_ip")

        if loadbalancerIPAddress == "" {
	    t.Fatal("Cannot retrieve the public IP address value for the load balancer.")
	}

    })

    test_structure.RunTestStage(t, "teardown", func() {
	terraformOptions := test_structure.LoadTerraformOptions(t, fixtureFolder)
	terraform.Destroy(t, terraformOptions)
    })
}

........ ....:

. go test -v ./ -timeout 30m -folder 'b1guvpc4h7oehnd9pabt'
=== RUN   TestEndToEndDeploymentScenario
TestEndToEndDeploymentScenario 2021-09-10T23:06:08+03:00 test_structure.go:29: The 'SKIP_setup' environment variable is set, so skipping stage 'setup'.
TestEndToEndDeploymentScenario 2021-09-10T23:06:08+03:00 test_structure.go:26: The 'SKIP_validate' environment variable is not set, so executing stage 'validate'.
Run some tests...
TestEndToEndDeploymentScenario 2021-09-10T23:06:08+03:00 save_test_data.go:215: Loading test data from ../.test-data/TerraformOptions.json
TestEndToEndDeploymentScenario 2021-09-10T23:06:08+03:00 retry.go:91: terraform [output -no-color -json load_balancer_public_ip]
TestEndToEndDeploymentScenario 2021-09-10T23:06:08+03:00 logger.go:66: Running command terraform with args [output -no-color -json load_balancer_public_ip]
TestEndToEndDeploymentScenario 2021-09-10T23:06:08+03:00 logger.go:66: "130.193.34.29"
TestEndToEndDeploymentScenario 2021-09-10T23:06:08+03:00 test_structure.go:29: The 'SKIP_teardown' environment variable is set, so skipping stage 'teardown'.
--- PASS: TestEndToEndDeploymentScenario (0.22s)
PASS
ok  	test	0.488s

........ ........, ... ..... Run some tests... ......... ......, ........... ........... ........ .. ....... IP ...............

...., ...... .... .. ......., ....... ........ . ..... ........ ..... - ........ ........... ........... .. ssh.

. ... .. ............ ........... . ........... ......, .. terratest-. ...-.. .... ........ ......... ...., ... ...... ........ .. . ..... ........ ......... ............

... ..... .. ....... ........... ........ ... ...... ..... - .... . .......... ......

..... ........... ..........

var folder = ...

....... ... ....

var sshKeyPath = flag.String("ssh-key-pass", "", "Private ssh key for access to virtual machines")

..... .. ............, ... ... ....... ...... .. ..... ssh-key-pass ..... ....... .... . .......... ......

., ..... .....

        if loadbalancerIPAddress == "" {
	    t.Fatal("Cannot retrieve the public IP address value for the load balancer.")
	}

....... .......... ....... ....

	// test ssh connect
	vmLinuxPublicIPAddress := terraform.Output(t, terraformOptions, "vm_linux_public_ip_address")

	key, err := ioutil.ReadFile(*sshKeyPath)
	if err != nil {
	    t.Fatalf("Unable to read private key: %v", err)
	}

	signer, err := ssh.ParsePrivateKey(key)
	if err != nil {
	    t.Fatalf("Unable to parse private key: %v", err)
	}

	sshConfig := &ssh.ClientConfig{
	    User: "ubuntu",
	    Auth: []ssh.AuthMethod{
		ssh.PublicKeys(signer),
	    },
	    HostKeyCallback: ssh.InsecureIgnoreHostKey(),
	}

	sshConnection, err := ssh.Dial("tcp", fmt.Sprintf("%s:22", vmLinuxPublicIPAddress), sshConfig)
	if err != nil {
	    t.Fatalf("Cannot establish SSH connection to vm-linux public IP address: %v", err)
	}

	defer sshConnection.Close()
        
	sshSession, err := sshConnection.NewSession()
	if err != nil {
	    t.Fatalf("Cannot create SSH session to vm-linux public IP address: %v", err)
	}

	defer sshSession.Close()
        
	err = sshSession.Run(fmt.Sprintf("ping -c 1 8.8.8.8"))
	if err != nil {
	    t.Fatalf("Cannot ping 8.8.8.8: %v", err)
	}

........ ... .. ......

vmLinuxPublicIPAddress := terraform.Output(t, terraformOptions, "vm_linux_public_ip_address")

.. output-.......... .......... .. ........ IP-..... ........... .......

key, err := ioutil.ReadFile(*sshKeyPath)
	if err != nil {
	    t.Fatalf("Unable to read private key: %v", err)
	}

. .......... key ......... .......... ..... . ......... .......

	signer, err := ssh.ParsePrivateKey(key)
	if err != nil {
	    t.Fatalf("Unable to parse private key: %v", err)
	}

........... .......... signer ........ .......... ......

	sshConfig := &ssh.ClientConfig{
	    User: "ubuntu",
	    Auth: []ssh.AuthMethod{
		ssh.PublicKeys(signer),
	    },
	    HostKeyCallback: ssh.InsecureIgnoreHostKey(),
	}

........... ............ ........... - ... ..... ......, . ..... ...... ...... ....... . ........... .......

	sshConnection, err := ssh.Dial("tcp", fmt.Sprintf("%s:22", vmLinuxPublicIPAddress), sshConfig)
	if err != nil {
	    t.Fatalf("Cannot establish SSH connection to vm-linux public IP address: %v", err)
	}

	defer sshConnection.Close()
        
	sshSession, err := sshConnection.NewSession()
	if err != nil {
	    t.Fatalf("Cannot create SSH session to vm-linux public IP address: %v", err)
	}

	defer sshSession.Close()

........ ............ .. ssh . ........... ....... . .. ........ ......... .......... (... defer)!

	err = sshSession.Run(fmt.Sprintf("ping -c 1 8.8.8.8"))
	if err != nil {
	    t.Fatalf("Cannot ping 8.8.8.8: %v", err)
	}

.. . ..... .... .. ......, ........ ...-...... ............ . ........... .......

. . imports ........... ... .... .......... - "golang.org/x/crypto/ssh"

....., ......... ... end2end_test.go .........:

package test

import (
    "fmt"
    "io/ioutil"
    "flag"
    "testing"

    "github.com/gruntwork-io/terratest/modules/terraform"
    test_structure "github.com/gruntwork-io/terratest/modules/test-structure"
    "golang.org/x/crypto/ssh"
)

var folder = flag.String("folder", "", "Folder ID in Yandex.Cloud")
var sshKeyPath = flag.String("ssh-key-pass", "", "Private ssh key for access to virtual machines")

func TestEndToEndDeploymentScenario(t *testing.T) {
    fixtureFolder := "../"

    test_structure.RunTestStage(t, "setup", func() {
	terraformOptions := &terraform.Options{
	    TerraformDir: fixtureFolder,

	    Vars: map[string]interface{}{
	    "yc_folder":    *folder,
	    },
        }

	test_structure.SaveTerraformOptions(t, fixtureFolder, terraformOptions)

	terraform.InitAndApply(t, terraformOptions)
    })

    test_structure.RunTestStage(t, "validate", func() {
        fmt.Println("Run some tests...")

        terraformOptions := test_structure.LoadTerraformOptions(t, fixtureFolder)

        // test load balancer ip existing
        loadbalancerIPAddress := terraform.Output(t, terraformOptions, "load_balancer_public_ip")

        if loadbalancerIPAddress == "" {
	    t.Fatal("Cannot retrieve the public IP address value for the load balancer.")
	}

	// test ssh connect
	vmLinuxPublicIPAddress := terraform.Output(t, terraformOptions, "vm_linux_public_ip_address")

	key, err := ioutil.ReadFile(*sshKeyPath)
	if err != nil {
	    t.Fatalf("Unable to read private key: %v", err)
	}

	signer, err := ssh.ParsePrivateKey(key)
	if err != nil {
	    t.Fatalf("Unable to parse private key: %v", err)
	}

	sshConfig := &ssh.ClientConfig{
	    User: "ubuntu",
	    Auth: []ssh.AuthMethod{
		ssh.PublicKeys(signer),
	    },
	    HostKeyCallback: ssh.InsecureIgnoreHostKey(),
	}

	sshConnection, err := ssh.Dial("tcp", fmt.Sprintf("%s:22", vmLinuxPublicIPAddress), sshConfig)
	if err != nil {
	    t.Fatalf("Cannot establish SSH connection to vm-linux public IP address: %v", err)
	}

	defer sshConnection.Close()

	sshSession, err := sshConnection.NewSession()
	if err != nil {
	    t.Fatalf("Cannot create SSH session to vm-linux public IP address: %v", err)
	}

	defer sshSession.Close()

	err = sshSession.Run(fmt.Sprintf("ping -c 1 8.8.8.8"))
	if err != nil {
	    t.Fatalf("Cannot ping 8.8.8.8: %v", err)
	}
    })

    test_structure.RunTestStage(t, "teardown", func() {
	terraformOptions := test_structure.LoadTerraformOptions(t, fixtureFolder)
	terraform.Destroy(t, terraformOptions)
    })
}

... ... .. ........ ..... .........., .. .. ........ .........

. go mod vendor

..... ......... ...., . ........ ........ .. .............. .... - -ssh-key-pass

. go test -v ./ -timeout 30m -folder 'b1guvpc4h7oehnd9pabt' -ssh-key-pass '/Users/igor/.ssh/yc'

=== RUN   TestEndToEndDeploymentScenario
TestEndToEndDeploymentScenario 2021-09-10T23:30:46+03:00 test_structure.go:29: The 'SKIP_setup' environment variable is set, so skipping stage 'setup'.
TestEndToEndDeploymentScenario 2021-09-10T23:30:46+03:00 test_structure.go:26: The 'SKIP_validate' environment variable is not set, so executing stage 'validate'.
Run some tests...
TestEndToEndDeploymentScenario 2021-09-10T23:30:46+03:00 save_test_data.go:215: Loading test data from ../.test-data/TerraformOptions.json
TestEndToEndDeploymentScenario 2021-09-10T23:30:46+03:00 retry.go:91: terraform [output -no-color -json load_balancer_public_ip]
TestEndToEndDeploymentScenario 2021-09-10T23:30:46+03:00 logger.go:66: Running command terraform with args [output -no-color -json load_balancer_public_ip]
TestEndToEndDeploymentScenario 2021-09-10T23:30:47+03:00 logger.go:66: "130.193.34.29"
TestEndToEndDeploymentScenario 2021-09-10T23:30:47+03:00 retry.go:91: terraform [output -no-color -json vm_linux_public_ip_address]
TestEndToEndDeploymentScenario 2021-09-10T23:30:47+03:00 logger.go:66: Running command terraform with args [output -no-color -json vm_linux_public_ip_address]
TestEndToEndDeploymentScenario 2021-09-10T23:30:47+03:00 logger.go:66: "84.201.159.187"
TestEndToEndDeploymentScenario 2021-09-10T23:30:47+03:00 test_structure.go:29: The 'SKIP_teardown' environment variable is set, so skipping stage 'teardown'.
--- PASS: TestEndToEndDeploymentScenario (1.34s)
PASS
ok  	test	1.772s

... ..... ...... ........

....... .. ..........

......... ..... ......... ....... . .... .......

. ......, ........ ...... .... ........... .... .......

....... ...... .... ....... ...............

... ..... ...... .......... ......... SKIP_teardown:

. unset SKIP_teardown

..... ........ ..... . .. .... ... ..... .. .......... ..... ......... ........ ..............:

. go test -v ./ -timeout 30m -folder 'b1guvpc4h7oehnd9pabt' -ssh-key-pass '/Users/igor/.ssh/yc'

...... ...... .......... .......... ......... SKIP_

. env | grep SKIP
SKIP_setup=true

. unset SKIP_setup

. ........ ...... .... ............

. go test -v ./ -timeout 30m -folder 'b1guvpc4h7oehnd9pabt' -ssh-key-pass '/Users/igor/.ssh/yc'

........ ..

... ........ .. .......... ............ ...... .. ........ ........... . ........... .......... . ......... . ........

. ........... ...... ......... .... README.md . ......... ........... .......

............. .......... ......... . .. ... ....... .......... .............
Last changed by  
.
Otus

7
Published on HackMD
