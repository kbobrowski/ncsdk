FROM ncsdk_base_dl

# configure environment variables
ENV NCSDK_VERSION=2.05.00.02
ENV INSTALL_DIR=/opt/movidius

ENV DIR=$INSTALL_DIR/NCSDK
ENV SDK_DIR=$DIR/ncsdk-x86_64
ENV SYS_INSTALL_DIR=/usr/local

# install sdk and api
RUN mkdir -p ${INSTALL_DIR} && \
    mkdir -p ${SYS_INSTALL_DIR}/include/mvnc2 && \
    mkdir -p ${SYS_INSTALL_DIR}/lib/mvnc

RUN wget --no-cache -O ${INSTALL_DIR}/ncsdk.tar.gz https://downloadmirror.intel.com/27839/eng/NCSDK-2.05.00.02.tar.gz

WORKDIR ${INSTALL_DIR}

RUN tar -zxf ./ncsdk.tar.gz && \
    mv NCSDK-${NCSDK_VERSION} NCSDK

RUN cp -r ${SDK_DIR}/tk ${SYS_INSTALL_DIR}/bin/ncsdk && \
    ln -s ${SYS_INSTALL_DIR}/bin/ncsdk/mvNCCompile.py ${SYS_INSTALL_DIR}/bin/mvNCCompile && \
    ln -s ${SYS_INSTALL_DIR}/bin/ncsdk/mvNCCheck.py ${SYS_INSTALL_DIR}/bin/mvNCCheck && \
    ln -s ${SYS_INSTALL_DIR}/bin/ncsdk/mvNCProfile.py ${SYS_INSTALL_DIR}/bin/mvNCProfile && \
    cp $SDK_DIR/fw/MvNCAPI-*.mvcmd $SYS_INSTALL_DIR/lib/mvnc && \
    cp $SDK_DIR/api/c/mvnc.h $SYS_INSTALL_DIR/include/mvnc2 && \
    cp $SDK_DIR/api/c/libmvnc.so.0 $SYS_INSTALL_DIR/lib/mvnc/ && \
    ln -s $SYS_INSTALL_DIR/include/mvnc2/mvnc.h $SYS_INSTALL_DIR/include/mvnc.h && \
    ln -s $SYS_INSTALL_DIR/lib/mvnc/libmvnc.so.0 $SYS_INSTALL_DIR/lib/libmvnc.so.0 && \
    ln -s $SYS_INSTALL_DIR/lib/mvnc/libmvnc.so.0 $SYS_INSTALL_DIR/lib/libmvnc.so && \
    ldconfig && \
    cp -r $DIR/version.txt $INSTALL_DIR/ && \
    cp -r $SDK_DIR/LICENSE $INSTALL_DIR/

RUN pip3 install --quiet --upgrade --force-reinstall $SDK_DIR/api && \
    pip2 install --quiet --upgrade --force-reinstall $SDK_DIR/api

# finalize installer
RUN cp $SDK_DIR/udev/97-usbboot.rules /etc/udev/rules.d/ && \
    # udevadm control --reload-rules && \
    udevadm trigger && \
    usermod -a -G users root && \
    rm $INSTALL_DIR/ncsdk.tar.gz

COPY ./ /ncsdk
WORKDIR /ncsdk

