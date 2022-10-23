# Begin Protobuf Definitions
set(PROTO_FILES
        duration.proto
        timestamp.proto
        basic_types.proto
        transaction.proto
        transaction_body.proto
        transaction_contents.proto
        ethereum_transaction.proto
        consensus_create_topic.proto
        consensus_delete_topic.proto
        consensus_get_topic_info.proto
        consensus_service.proto
        consensus_submit_message.proto
        consensus_topic_info.proto
        consensus_update_topic.proto
        contract_call.proto
        contract_call_local.proto
        contract_create.proto
        contract_delete.proto
        contract_get_bytecode.proto
        contract_get_info.proto
        contract_get_records.proto
        contract_update.proto
        crypto_add_live_hash.proto
        crypto_approve_allowance.proto
        crypto_create.proto
        crypto_delete.proto
        crypto_delete_allowance.proto
        crypto_delete_live_hash.proto
        crypto_get_account_balance.proto
        crypto_get_account_records.proto
        crypto_get_info.proto
        crypto_get_live_hash.proto
        crypto_get_stakers.proto
        crypto_service.proto
        crypto_transfer.proto
        crypto_update.proto
        custom_fees.proto
        exchange_rate.proto
        file_append.proto
        file_create.proto
        file_delete.proto
        file_get_contents.proto
        file_get_info.proto
        file_service.proto
        file_update.proto
        freeze.proto
        freeze_service.proto
        freeze_type.proto
        get_account_details.proto
        get_by_key.proto
        get_by_solidity_id.proto
        network_get_execution_time.proto
        network_get_version_info.proto
        network_service.proto
        node_stake_update.proto
        query.proto
        query_header.proto
        response.proto
        response_code.proto
        response_header.proto
        schedulable_transaction_body.proto
        schedule_create.proto
        schedule_delete.proto
        schedule_get_info.proto
        schedule_service.proto
        schedule_sign.proto
        smart_contract_service.proto
        system_delete.proto
        system_undelete.proto
        throttle_definitions.proto
        token_associate.proto
        token_burn.proto
        token_create.proto
        token_delete.proto
        token_dissociate.proto
        token_fee_schedule_update.proto
        token_freeze_account.proto
        token_get_account_nft_infos.proto
        token_get_info.proto
        token_get_nft_info.proto
        token_get_nft_infos.proto
        token_grant_kyc.proto
        token_mint.proto
        token_pause.proto
        token_revoke_kyc.proto
        token_service.proto
        token_unfreeze_account.proto
        token_unpause.proto
        token_update.proto
        token_wipe_account.proto
        transaction_get_fast_record.proto
        transaction_get_receipt.proto
        transaction_get_record.proto
        transaction_receipt.proto
        transaction_record.proto
        transaction_response.proto
        unchecked_submit.proto
        util_prng.proto
        util_service.proto)
# End Protobuf Definitions

add_library(hapi STATIC ${PROTO_FILES})
target_link_libraries(hapi PUBLIC ${Protobuf_LIBRARIES} gRPC::grpc gRPC::grpc++)
target_include_directories(hapi PUBLIC ${CMAKE_CURRENT_BINARY_DIR})

#
# Compile protobuf and grpc files in the hapi target to cpp
#
get_target_property(grpc_cpp_plugin_location gRPC::grpc_cpp_plugin LOCATION)
protobuf_generate(TARGET hapi LANGUAGE cpp IMPORT_DIRS ${PROTO_SRC})
protobuf_generate(TARGET hapi LANGUAGE grpc GENERATE_EXTENSIONS .grpc.pb.h .grpc.pb.cc PLUGIN "protoc-gen-grpc=${grpc_cpp_plugin_location}" IMPORT_DIRS ${PROTO_SRC})

# Install Library & Headers
install(TARGETS hapi ARCHIVE DESTINATION lib)

install(FILES ${ZLIB_LIBRARY_RELEASE} DESTINATION lib)
install(FILES ${OPENSSL_SSL_LIBRARIES} DESTINATION lib)
install(FILES ${OPENSSL_CRYPTO_LIBRARIES} DESTINATION lib)
install(FILES ${Protobuf_LIBRARY_RELEASE} DESTINATION lib)

get_target_property(gRPC_LIBRARY gRPC::grpc LOCATION)
install(FILES ${gRPC_LIBRARY} DESTINATION lib)

get_target_property(gRPC_CPP_LIBRARY gRPC::grpc++ LOCATION)
install(FILES ${gRPC_CPP_LIBRARY} DESTINATION lib)

get_target_property(RE2_LIBRARY re2::re2 LOCATION)
install(FILES ${RE2_LIBRARY} DESTINATION lib)

get_target_property(CARES_LIBRARY c-ares::cares LOCATION)
install(FILES ${CARES_LIBRARY} DESTINATION lib)

get_target_property(UPB_LIBRARY upb::upb LOCATION)
install(FILES ${UPB_LIBRARY} DESTINATION lib)

get_target_property(ABSL_BASE_LIBRARY absl::base LOCATION)
get_target_property(ABSL_STRINGS_LIBRARY absl::strings LOCATION)
get_target_property(ABSL_HASH_LIBRARY absl::hash LOCATION)
get_target_property(ABSL_TIME_LIBRARY absl::time LOCATION)
get_target_property(ABSL_STATUS_LIBRARY absl::status LOCATION)
get_target_property(ABSL_FLAGS_LIBRARY absl::flags LOCATION)
get_target_property(ABSL_SYNCHRONIZATION_LIBRARY absl::synchronization LOCATION)
install(FILES
            ${ABSL_BASE_LIBRARY}
            ${ABSL_ALGORITHM_LIBRARY}
            ${ABSL_STRINGS_LIBRARY}
            ${ABSL_HASH_LIBRARY}
            ${ABSL_TIME_LIBRARY}
            ${ABSL_STATUS_LIBRARY}
            ${ABSL_FLAGS_LIBRARY}
            ${ABSL_SYNCHRONIZATION_LIBRARY}
        DESTINATION lib)

install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR} DESTINATION include FILES_MATCHING PATTERN "*.h")
install(DIRECTORY ${Protobuf_INCLUDE_DIRS} DESTINATION ${CMAKE_INSTALL_PREFIX})
install(DIRECTORY ${ZLIB_INCLUDE_DIRS} DESTINATION ${CMAKE_INSTALL_PREFIX})