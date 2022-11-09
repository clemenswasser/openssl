# testutil
add_library(testutil
    ${OpenSSL_SOURCE_DIR}/apps/lib/opt.c
    ${OpenSSL_SOURCE_DIR}/apps/lib/win32_init.c
    testutil/apps_shims.c
    testutil/basic_output.c
    testutil/cb.c
    testutil/driver.c
    testutil/fake_random.c
    testutil/format_output.c
    testutil/load.c
    testutil/main.c
    testutil/options.c
    testutil/output.c
    testutil/provider.c
    testutil/random.c
    testutil/stanza.c
    testutil/test_cleanup.c
    testutil/test_options.c
    testutil/tests.c
    testutil/testutil_init.c)
target_link_libraries(testutil PRIVATE openssl_defaults)
target_include_directories(testutil PUBLIC ${OpenSSL_SOURCE_DIR}/apps/include)

# Crypto tests
function(add_crypto_tests)
    foreach(TEST_NAME IN LISTS ARGN)
        add_executable("${TEST_NAME}" "${TEST_NAME}.c")
        target_link_libraries("${TEST_NAME}" PRIVATE crypto testutil openssl_defaults)
        add_test(NAME "${TEST_NAME}" COMMAND "${TEST_NAME}")
    endforeach()
endfunction()

add_crypto_tests(
    aborttest
    aesgcmtest
    afalgtest
    algorithmid_test
    asn1_decode_test
    asn1_dsa_internal_test
    asn1_encode_test
    asn1_internal_test
    asn1_string_table_test
    asn1_time_test
    asynctest
    bftest
    bio_callback_test
    bio_core_test
    bio_dgram_test
    bio_enc_test
    bio_memleak_test
    bio_prefix_text
    bio_readbuffer_test
    bio_tfo_test
    bioprinttest
    bn_internal_test
    bntest
    casttest
    chacha_internal_test
    cmactest
    cmp_asn_test
    cmp_client_test
    cmp_ctx_test
    cmp_hdr_test
    cmp_msg_test
    cmp_protect_test
    cmp_server_test
    cmp_status_test
    cmp_vfy_test
    cmsapitest
    conf_include_test
    confdump
    constant_time_test
    context_internal_test
    crltest
    ct_test
    ctype_internal_test
    curve448_internal_test
    d2i_test
    defltfips_test
    destest
    dhtest
    drbgtest
    dsa_no_digest_size_test
    dsatest
    ec_internal_test
    ecdsatest
    ecstresstest
    ectest
    endecode_test
    endecoder_legacy_test
    enginetest
    errtest
    evp_extra_test
    evp_extra_test2
    evp_fetch_prov_test
    evp_kdf_test
    evp_libctx_test
    evp_pkey_ctx_new_from_name
    evp_pkey_dhkem_test
    evp_pkey_dparams_test
    evp_pkey_provided_test
    evp_test
    exdatatest
    exptest
    ffc_internal_test
    fips_version_test
    gmdifftest
    hexstr_test
    hmactest
    http_test
    ideatest
    igetest
    keymgmt_internal_test
    lhash_test
    list_test
    localetest
    mdc2_internal_test
    mdc2test
    membio_test
    memleaktest
    modes_internal_test
    moduleloadtest
    namemap_internal_test
    ocspapitest
    ossl_store_test
    packettest
    param_build_test
    params_api_test
    params_conversion_test
    params_test
    pbelutest
    pbetest
    pem_read_depr_test
    pemtest
    pkcs12_api_test
    pkcs12_format_test
    pkcs7_test
    pkey_meth_kdf_test
    pkey_meth_test
    poly1305_internal_test
    property_test
    prov_config_test
    provfetchtest
    provider_fallback_test
    provider_internal_test
    provider_pkey_test
    provider_status_test
    provider_test
    punycode_test
    rand_status_test
    rand_test
    rc2test
    rc4test
    rc5test
    rdcpu_sanitytest
    rsa_complex
    rsa_mp_test
    rsa_sp800_56b_test
    rsa_test
    safe_math_test
    sanitytest
    secmemtest
    sha_test
    shlibloadtest
    siphash_internal_test
    sm2_internal_test
    sm3_internal_test
    sm4_internal_test
    sparse_array_test
    srptest
    ssl_cert_table_internal_test
    stack_test
    test_test
    threadstest
    threadstest_fips
    time_offset_test
    timing_load_creds
    trace_api_test
    upcallstest
    user_property_test
    v3ext
    v3nametest
    verify_extra_test
    versions
    x509_check_cert_pkey_test
    x509_dup_cert_test
    x509_internal_test
    x509_test
    x509_time_test
    x509aux)

target_sources(cmp_client_test PRIVATE ${OpenSSL_SOURCE_DIR}/apps/lib/cmp_mock_srv.c)
target_sources(pkcs12_format_test PRIVATE helpers/pkcs12.c)
target_sources(provider_pkey_test PRIVATE fake_rsaprov.c)

target_include_directories(bn_internal_test PRIVATE ${OpenSSL_SOURCE_DIR}/crypto/bn)
target_include_directories(curve448_internal_test PRIVATE ${OpenSSL_SOURCE_DIR}/crypto/ec/curve448)
target_include_directories(ec_internal_test PRIVATE ${OpenSSL_SOURCE_DIR}/crypto/ec)
target_include_directories(rsa_sp800_56b_test PRIVATE ${OpenSSL_SOURCE_DIR}/crypto/rsa)

add_library(simpledynamic OBJECT simpledynamic.c)
target_include_directories(simpledynamic PRIVATE ${OpenSSL_SOURCE_DIR}/include)
target_link_libraries(moduleloadtest PRIVATE simpledynamic)
target_link_libraries(shlibloadtest PRIVATE simpledynamic)

add_library(cmp_testlib OBJECT helpers/cmp_testlib.c)
target_include_directories(cmp_testlib PRIVATE
    ${OpenSSL_SOURCE_DIR}/include
    ${OpenSSL_SOURCE_DIR}/apps/include)
target_link_libraries(cmp_asn_test PRIVATE cmp_testlib)
target_link_libraries(cmp_client_test PRIVATE cmp_testlib)
target_link_libraries(cmp_ctx_test PRIVATE cmp_testlib)
target_link_libraries(cmp_hdr_test PRIVATE cmp_testlib)
target_link_libraries(cmp_msg_test PRIVATE cmp_testlib)
target_link_libraries(cmp_protect_test PRIVATE cmp_testlib)
target_link_libraries(cmp_server_test PRIVATE cmp_testlib)
target_link_libraries(cmp_status_test PRIVATE cmp_testlib)
target_link_libraries(cmp_vfy_test PRIVATE cmp_testlib)

add_library(predefined_dhparams OBJECT helpers/predefined_dhparams.c)
target_include_directories(predefined_dhparams PRIVATE ${OpenSSL_SOURCE_DIR}/include)
target_link_libraries(endecode_test PRIVATE predefined_dhparams)

# target_link_libraries(ssl_old_test PRIVATE predefined_dhparams)
add_library(p_test OBJECT p_test.c)
target_include_directories(p_test PRIVATE ${OpenSSL_SOURCE_DIR}/include)
target_compile_definitions(p_test PUBLIC PROVIDER_INIT_FUNCTION_NAME=p_test_init)
target_link_libraries(provider_test PRIVATE p_test)
target_link_libraries(provider_internal_test PRIVATE p_test)

# SSL Tests
function(add_ssl_tests)
    add_crypto_tests("${ARGN}")

    foreach(TEST_NAME IN LISTS ARGN)
        target_link_libraries("${TEST_NAME}" PRIVATE ssl)
    endforeach()
endfunction()

# test\asynciotest.exe: test\asynciotest-bin-asynciotest.obj test\helpers\asynciotest-bin-ssltestlib.obj libssl.lib test\libtestutil.lib libcrypto.lib
# test\bad_dtls_test.exe: test\bad_dtls_test-bin-bad_dtls_test.obj libssl.lib test\libtestutil.lib libcrypto.lib
# test\build_wincrypt_test.exe: test\build_wincrypt_test-bin-build_wincrypt_test.obj libssl.lib libcrypto.lib
# test\buildtest_c_aes.exe: test\buildtest_c_aes-bin-buildtest_aes.obj libssl.lib libcrypto.lib
# test\buildtest_c_async.exe: test\buildtest_c_async-bin-buildtest_async.obj libssl.lib libcrypto.lib
# test\buildtest_c_blowfish.exe: test\buildtest_c_blowfish-bin-buildtest_blowfish.obj libssl.lib libcrypto.lib
# test\buildtest_c_bn.exe: test\buildtest_c_bn-bin-buildtest_bn.obj libssl.lib libcrypto.lib
# test\buildtest_c_buffer.exe: test\buildtest_c_buffer-bin-buildtest_buffer.obj libssl.lib libcrypto.lib
# test\buildtest_c_camellia.exe: test\buildtest_c_camellia-bin-buildtest_camellia.obj libssl.lib libcrypto.lib
# test\buildtest_c_cast.exe: test\buildtest_c_cast-bin-buildtest_cast.obj libssl.lib libcrypto.lib
# test\buildtest_c_cmac.exe: test\buildtest_c_cmac-bin-buildtest_cmac.obj libssl.lib libcrypto.lib
# test\buildtest_c_cmp_util.exe: test\buildtest_c_cmp_util-bin-buildtest_cmp_util.obj libssl.lib libcrypto.lib
# test\buildtest_c_comp.exe: test\buildtest_c_comp-bin-buildtest_comp.obj libssl.lib libcrypto.lib
# test\buildtest_c_conf_api.exe: test\buildtest_c_conf_api-bin-buildtest_conf_api.obj libssl.lib libcrypto.lib
# test\buildtest_c_conftypes.exe: test\buildtest_c_conftypes-bin-buildtest_conftypes.obj libssl.lib libcrypto.lib
# test\buildtest_c_core.exe: test\buildtest_c_core-bin-buildtest_core.obj libssl.lib libcrypto.lib
# test\buildtest_c_core_dispatch.exe: test\buildtest_c_core_dispatch-bin-buildtest_core_dispatch.obj libssl.lib libcrypto.lib
# test\buildtest_c_core_names.exe: test\buildtest_c_core_names-bin-buildtest_core_names.obj libssl.lib libcrypto.lib
# test\buildtest_c_core_object.exe: test\buildtest_c_core_object-bin-buildtest_core_object.obj libssl.lib libcrypto.lib
# test\buildtest_c_cryptoerr_legacy.exe: test\buildtest_c_cryptoerr_legacy-bin-buildtest_cryptoerr_legacy.obj libssl.lib libcrypto.lib
# test\buildtest_c_decoder.exe: test\buildtest_c_decoder-bin-buildtest_decoder.obj libssl.lib libcrypto.lib
# test\buildtest_c_des.exe: test\buildtest_c_des-bin-buildtest_des.obj libssl.lib libcrypto.lib
# test\buildtest_c_dh.exe: test\buildtest_c_dh-bin-buildtest_dh.obj libssl.lib libcrypto.lib
# test\buildtest_c_dsa.exe: test\buildtest_c_dsa-bin-buildtest_dsa.obj libssl.lib libcrypto.lib
# test\buildtest_c_dtls1.exe: test\buildtest_c_dtls1-bin-buildtest_dtls1.obj libssl.lib libcrypto.lib
# test\buildtest_c_e_os2.exe: test\buildtest_c_e_os2-bin-buildtest_e_os2.obj libssl.lib libcrypto.lib
# test\buildtest_c_ebcdic.exe: test\buildtest_c_ebcdic-bin-buildtest_ebcdic.obj libssl.lib libcrypto.lib
# test\buildtest_c_ec.exe: test\buildtest_c_ec-bin-buildtest_ec.obj libssl.lib libcrypto.lib
# test\buildtest_c_ecdh.exe: test\buildtest_c_ecdh-bin-buildtest_ecdh.obj libssl.lib libcrypto.lib
# test\buildtest_c_ecdsa.exe: test\buildtest_c_ecdsa-bin-buildtest_ecdsa.obj libssl.lib libcrypto.lib
# test\buildtest_c_encoder.exe: test\buildtest_c_encoder-bin-buildtest_encoder.obj libssl.lib libcrypto.lib
# test\buildtest_c_engine.exe: test\buildtest_c_engine-bin-buildtest_engine.obj libssl.lib libcrypto.lib
# test\buildtest_c_evp.exe: test\buildtest_c_evp-bin-buildtest_evp.obj libssl.lib libcrypto.lib
# test\buildtest_c_fips_names.exe: test\buildtest_c_fips_names-bin-buildtest_fips_names.obj libssl.lib libcrypto.lib
# test\buildtest_c_hmac.exe: test\buildtest_c_hmac-bin-buildtest_hmac.obj libssl.lib libcrypto.lib
# test\buildtest_c_http.exe: test\buildtest_c_http-bin-buildtest_http.obj libssl.lib libcrypto.lib
# test\buildtest_c_idea.exe: test\buildtest_c_idea-bin-buildtest_idea.obj libssl.lib libcrypto.lib
# test\buildtest_c_kdf.exe: test\buildtest_c_kdf-bin-buildtest_kdf.obj libssl.lib libcrypto.lib
# test\buildtest_c_macros.exe: test\buildtest_c_macros-bin-buildtest_macros.obj libssl.lib libcrypto.lib
# test\buildtest_c_md4.exe: test\buildtest_c_md4-bin-buildtest_md4.obj libssl.lib libcrypto.lib
# test\buildtest_c_md5.exe: test\buildtest_c_md5-bin-buildtest_md5.obj libssl.lib libcrypto.lib
# test\buildtest_c_mdc2.exe: test\buildtest_c_mdc2-bin-buildtest_mdc2.obj libssl.lib libcrypto.lib
# test\buildtest_c_modes.exe: test\buildtest_c_modes-bin-buildtest_modes.obj libssl.lib libcrypto.lib
# test\buildtest_c_obj_mac.exe: test\buildtest_c_obj_mac-bin-buildtest_obj_mac.obj libssl.lib libcrypto.lib
# test\buildtest_c_objects.exe: test\buildtest_c_objects-bin-buildtest_objects.obj libssl.lib libcrypto.lib
# test\buildtest_c_ossl_typ.exe: test\buildtest_c_ossl_typ-bin-buildtest_ossl_typ.obj libssl.lib libcrypto.lib
# test\buildtest_c_param_build.exe: test\buildtest_c_param_build-bin-buildtest_param_build.obj libssl.lib libcrypto.lib
# test\buildtest_c_params.exe: test\buildtest_c_params-bin-buildtest_params.obj libssl.lib libcrypto.lib
# test\buildtest_c_pem.exe: test\buildtest_c_pem-bin-buildtest_pem.obj libssl.lib libcrypto.lib
# test\buildtest_c_pem2.exe: test\buildtest_c_pem2-bin-buildtest_pem2.obj libssl.lib libcrypto.lib
# test\buildtest_c_prov_ssl.exe: test\buildtest_c_prov_ssl-bin-buildtest_prov_ssl.obj libssl.lib libcrypto.lib
# test\buildtest_c_provider.exe: test\buildtest_c_provider-bin-buildtest_provider.obj libssl.lib libcrypto.lib
# test\buildtest_c_rand.exe: test\buildtest_c_rand-bin-buildtest_rand.obj libssl.lib libcrypto.lib
# test\buildtest_c_rc2.exe: test\buildtest_c_rc2-bin-buildtest_rc2.obj libssl.lib libcrypto.lib
# test\buildtest_c_rc4.exe: test\buildtest_c_rc4-bin-buildtest_rc4.obj libssl.lib libcrypto.lib
# test\buildtest_c_ripemd.exe: test\buildtest_c_ripemd-bin-buildtest_ripemd.obj libssl.lib libcrypto.lib
# test\buildtest_c_rsa.exe: test\buildtest_c_rsa-bin-buildtest_rsa.obj libssl.lib libcrypto.lib
# test\buildtest_c_seed.exe: test\buildtest_c_seed-bin-buildtest_seed.obj libssl.lib libcrypto.lib
# test\buildtest_c_self_test.exe: test\buildtest_c_self_test-bin-buildtest_self_test.obj libssl.lib libcrypto.lib
# test\buildtest_c_sha.exe: test\buildtest_c_sha-bin-buildtest_sha.obj libssl.lib libcrypto.lib
# test\buildtest_c_srtp.exe: test\buildtest_c_srtp-bin-buildtest_srtp.obj libssl.lib libcrypto.lib
# test\buildtest_c_ssl2.exe: test\buildtest_c_ssl2-bin-buildtest_ssl2.obj libssl.lib libcrypto.lib
# test\buildtest_c_sslerr_legacy.exe: test\buildtest_c_sslerr_legacy-bin-buildtest_sslerr_legacy.obj libssl.lib libcrypto.lib
# test\buildtest_c_stack.exe: test\buildtest_c_stack-bin-buildtest_stack.obj libssl.lib libcrypto.lib
# test\buildtest_c_store.exe: test\buildtest_c_store-bin-buildtest_store.obj libssl.lib libcrypto.lib
# test\buildtest_c_symhacks.exe: test\buildtest_c_symhacks-bin-buildtest_symhacks.obj libssl.lib libcrypto.lib
# test\buildtest_c_thread.exe: test\buildtest_c_thread-bin-buildtest_thread.obj libssl.lib libcrypto.lib
# test\buildtest_c_tls1.exe: test\buildtest_c_tls1-bin-buildtest_tls1.obj libssl.lib libcrypto.lib
# test\buildtest_c_ts.exe: test\buildtest_c_ts-bin-buildtest_ts.obj libssl.lib libcrypto.lib
# test\buildtest_c_txt_db.exe: test\buildtest_c_txt_db-bin-buildtest_txt_db.obj libssl.lib libcrypto.lib
# test\buildtest_c_types.exe: test\buildtest_c_types-bin-buildtest_types.obj libssl.lib libcrypto.lib
# test\buildtest_c_whrlpool.exe: test\buildtest_c_whrlpool-bin-buildtest_whrlpool.obj libssl.lib libcrypto.lib
# test\ca_internals_test.exe: apps\ca_internals_test-bin-ca.obj apps\lib\ca_internals_test-bin-app_libctx.obj apps\lib\ca_internals_test-bin-app_provider.obj apps\lib\ca_internals_test-bin-app_rand.obj apps\lib\ca_internals_test-bin-app_x509.obj apps\lib\ca_internals_test-bin-apps.obj apps\lib\ca_internals_test-bin-apps_ui.obj apps\lib\ca_internals_test-bin-engine.obj apps\lib\ca_internals_test-bin-fmt.obj crypto\asn1\ca_internals_test-bin-a_time.obj crypto\ca_internals_test-bin-ctype.obj test\ca_internals_test-bin-ca_internals_test.obj libssl.lib test\libtestutil.lib libcrypto.lib
# test\cipher_overhead_test.exe: test\cipher_overhead_test-bin-cipher_overhead_test.obj libssl_static.lib test\libtestutil.lib libcrypto_static.lib
# test\cipherbytes_test.exe: test\cipherbytes_test-bin-cipherbytes_test.obj libssl.lib test\libtestutil.lib libcrypto.lib
# test\cipherlist_test.exe: test\cipherlist_test-bin-cipherlist_test.obj libssl.lib test\libtestutil.lib libcrypto.lib
# test\ciphername_test.exe: test\ciphername_test-bin-ciphername_test.obj libssl.lib test\libtestutil.lib libcrypto.lib
# test\clienthellotest.exe: test\clienthellotest-bin-clienthellotest.obj libssl.lib test\libtestutil.lib libcrypto.lib
# test\danetest.exe: test\danetest-bin-danetest.obj libssl.lib test\libtestutil.lib libcrypto.lib
# test\dtls_mtu_test.exe: test\dtls_mtu_test-bin-dtls_mtu_test.obj test\helpers\dtls_mtu_test-bin-ssltestlib.obj libssl.lib test\libtestutil.lib libcrypto.lib
# test\dtlstest.exe: test\dtlstest-bin-dtlstest.obj test\helpers\dtlstest-bin-ssltestlib.obj libssl.lib test\libtestutil.lib libcrypto.lib
# test\dtlsv1listentest.exe: test\dtlsv1listentest-bin-dtlsv1listentest.obj libssl.lib test\libtestutil.lib libcrypto.lib
# test\ext_internal_test.exe: test\ext_internal_test-bin-ext_internal_test.obj libssl_static.lib test\libtestutil.lib libcrypto_static.lib
# test\fatalerrtest.exe: test\fatalerrtest-bin-fatalerrtest.obj test\helpers\fatalerrtest-bin-ssltestlib.obj libssl.lib test\libtestutil.lib libcrypto.lib
# test\recordlentest.exe: test\helpers\recordlentest-bin-ssltestlib.obj test\recordlentest-bin-recordlentest.obj libssl.lib test\libtestutil.lib libcrypto.lib
# test\servername_test.exe: test\helpers\servername_test-bin-ssltestlib.obj test\servername_test-bin-servername_test.obj libssl.lib test\libtestutil.lib libcrypto.lib
# test\ssl_ctx_test.exe: test\ssl_ctx_test-bin-ssl_ctx_test.obj libssl.lib test\libtestutil.lib libcrypto.lib
# test\ssl_old_test.exe: test\helpers\ssl_old_test-bin-predefined_dhparams.obj test\ssl_old_test-bin-ssl_old_test.obj libssl_static.lib test\libtestutil.lib libcrypto_static.lib
# test\ssl_test.exe: test\helpers\ssl_test-bin-handshake.obj test\helpers\ssl_test-bin-handshake_srp.obj test\helpers\ssl_test-bin-ssl_test_ctx.obj test\ssl_test-bin-ssl_test.obj libssl.lib test\libtestutil.lib libcrypto.lib
# test\ssl_test_ctx_test.exe: test\helpers\ssl_test_ctx_test-bin-ssl_test_ctx.obj test\ssl_test_ctx_test-bin-ssl_test_ctx_test.obj libssl.lib test\libtestutil.lib libcrypto.lib
# test\sslapitest.exe: test\helpers\sslapitest-bin-ssltestlib.obj test\sslapitest-bin-filterprov.obj test\sslapitest-bin-sslapitest.obj test\sslapitest-bin-tls-provider.obj libssl.lib test\libtestutil.lib libcrypto.lib
# test\sslbuffertest.exe: test\helpers\sslbuffertest-bin-ssltestlib.obj test\sslbuffertest-bin-sslbuffertest.obj libssl.lib test\libtestutil.lib libcrypto.lib
# test\sslcorrupttest.exe: test\helpers\sslcorrupttest-bin-ssltestlib.obj test\sslcorrupttest-bin-sslcorrupttest.obj libssl.lib test\libtestutil.lib libcrypto.lib
# test\sysdefaulttest.exe: test\sysdefaulttest-bin-sysdefaulttest.obj libssl.lib test\libtestutil.lib libcrypto.lib
# test\tls13ccstest.exe: test\helpers\tls13ccstest-bin-ssltestlib.obj test\tls13ccstest-bin-tls13ccstest.obj libssl.lib test\libtestutil.lib libcrypto.lib
# test\tls13encryptiontest.exe: test\tls13encryptiontest-bin-tls13encryptiontest.obj libssl_static.lib test\libtestutil.lib libcrypto_static.lib
# test\tls13secretstest.exe: crypto\tls13secretstest-bin-packet.obj crypto\tls13secretstest-bin-quic_vlint.obj ssl\tls13secretstest-bin-tls13_enc.obj test\tls13secretstest-bin-tls13secretstest.obj libssl.lib test\libtestutil.lib libcrypto.lib
# test\uitest.exe: apps\lib\uitest-bin-apps_ui.obj test\uitest-bin-uitest.obj libssl.lib test\libtestutil.lib libcrypto.lib
# test\wpackettest.exe: test\wpackettest-bin-wpackettest.obj libssl_static.lib test\libtestutil.lib libcrypto_static.lib