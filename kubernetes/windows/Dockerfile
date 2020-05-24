FROM mcr.microsoft.com/azuremonitor/containerinsights/ciprod:winakslogbase-04212020

SHELL ["powershell"]

ENV tmpdir /opt/omsagentwindows/scripts/powershell

WORKDIR /opt/omsagentwindows/scripts/powershell

# gangams - TBD -  build certificate generator code here itself so that we can avoid manual step
COPY ./omsagentwindows/certgenerator/* /opt/omsagentwindows/certgenerator/

COPY setup.ps1 /opt/omsagentwindows/scripts/powershell
RUN ./setup.ps1

COPY main.ps1 /opt/omsagentwindows/scripts/powershell
COPY ./scripts/powershell/filesystemwatcher.ps1 /opt/omsagentwindows/scripts/powershell
COPY ./scripts/cmd/livenessprobe.cmd /opt/omsagentwindows/scripts/cmd/

# copy ruby scripts to /opt folder
COPY ./scripts/ruby/*.rb /opt/omsagentwindows/scripts/ruby/

# gangams - TBD - figure out building the go code in the docker file or have Makefile do this
COPY ./omsagentwindows/out_oms.so /opt/omsagentwindows/out_oms.so

COPY ./fluent/fluent.conf /etc/fluent/
COPY ./fluent/rubyKeepCertificateAlive/*.rb /etc/fluent/plugin/
COPY ./fluent-bit/fluent-bit.conf /etc/fluent-bit
COPY ./omsagentwindows/out_oms.conf /etc/omsagentwindows

ENV AGENT_VERSION "winakslogprivatepreview"
ENV OS_TYPE "windows"
ENV APPLICATIONINSIGHTS_AUTH "OTQzNWI0M2YtOTdkNS00ZGVkLThkOTAtYjA0Nzk1OGU2ZTg3"
ENV AZMON_COLLECT_ENV False
ENV CI_CERT_LOCATION "C://oms.crt"
ENV CI_KEY_LOCATION "C://oms.key"

ENTRYPOINT ["powershell", "C:\\opt\\omsagentwindows\\scripts\\powershell\\main.ps1"]