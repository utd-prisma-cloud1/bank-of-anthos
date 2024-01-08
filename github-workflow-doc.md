# Prisma Cloud IaC Scan Github Action

This Github Action is used to run a scan on Kubernetes manifests using Prisma Cloud by Palo Alto Networks. The action runs on pull requests to the `main` branch.

## Workflow

When a pull request is opened on the `main` branch, this Github Action is triggered and performs the following tasks:

1. **Checkout**: This step checks out the source code of the pull request.
2. **Run Kubernetes Scan**: This step runs the Bridgecrew Github Action that performs a Kubernetes scan on the Kubernetes manifests in the repository using Prisma Cloud. 

   ```
   - name: Run Kubernetes Scan
     id: Bridgecrew
     uses: bridgecrewio/bridgecrew-action@master
     env:
       PRISMA_API_URL: https://api4.prismacloud.io
     with:
       api-key: ${{ secrets.PC_API_KEY }}
       directory: "${{ github.workspace }}/k8s-manifests"
       soft_fail: false
       framework: kubernetes
       output_format: cli,sarif
       output_file_path: console,results.sarif
       quiet: true
       log_level: WARNING
       skip_check: LOW
       check: CKV_K8S*
   ```
   The action is run with the following parameters:
      - `api-key`: The Prisma Cloud API key stored as a secret in the repository.
      - `directory`: Root directory to scan.
      - `soft_fail`: Runs checks without failing build. Defaults to `false`.
      - `framework`: The infrastructure as code framework to use for the scan. In this case, it is set to `kubernetes`.
      - `output_format`: The format for the scan results. In this case, it is set to `cli,sarif`.
      - `output_file_path`: The path to the output file. In this case, it is set to `console,results.sarif`.
      - `quiet`: If set to `true`, the action will not output passed checks and will only print failed checks. Defaults to `true`.
      - `log_level`: The log level for the action. In this case, it is set to `WARNING`.
      - `skip_check`: Filter scan to run on all check but a specific check identifier(blacklist), You can specify multiple checks separated by comma delimiter, clashes with check. In this case, it is set to `LOW`.
      - `check`: Filter scan to run only on a specific check identifier, You can specify multiple checks separated by comma delimiter. In this case, it is set to `CKV_K8S*`.
   
## Conclusion

This Github Action provides an easy and automated way to scan Kubernetes manifests for vulnerabilities using Prisma Cloud.
- Prisma Cloud provides an effective way to scan IaC for vulnerabilities and misconfigurations. 
- The GitHub Actions workflow we've demonstrated automates this process, allowing developers to identify and fix issues early in the software development lifecycle. 
- By integrating Prisma Cloud scans into your CI/CD pipeline, you can ensure that your IaC is secure and compliant before its deployed. 
- With the use of this GitHub Actions workflow and Prisma Cloud scans, you can have greater confidence in the security of your applications and reduce the risk of potential attacks.


## References:
- https://github.com/bridgecrewio/bridgecrew-action
