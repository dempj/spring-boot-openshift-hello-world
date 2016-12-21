FROM wildfly:10.0
RUN mkdir -p target/
COPY ROOT.war target/
ENTRYPOINT ["sh", "-c", "${STI_SCRIPTS_PATH}/assemble && ${STI_SCRIPTS_PATH}/run"]
