set (DNF_SRCS dnf-command.c dnf-utils.c)

glib_compile_resources (DNF_COMMAND_INSTALL plugins/install/dnf-command-install.gresource.xml
                        C_PREFIX dnf_command_install
                        INTERNAL)
list (APPEND DNF_COMMAND_INSTALL "plugins/install/dnf-command-install.c")

glib_compile_resources (DNF_COMMAND_REMOVE plugins/remove/dnf-command-remove.gresource.xml
                        C_PREFIX dnf_command_remove
                        INTERNAL)
list (APPEND DNF_COMMAND_REMOVE "plugins/remove/dnf-command-remove.c")

glib_compile_resources (DNF_COMMAND_UPDATE plugins/update/dnf-command-update.gresource.xml
                        C_PREFIX dnf_command_update
                        INTERNAL)
list (APPEND DNF_COMMAND_UPDATE "plugins/update/dnf-command-update.c")

glib_compile_resources (DNF_COMMAND_CLEAN plugins/clean/dnf-command-clean.gresource.xml
                        C_PREFIX dnf_command_clean
                        INTERNAL)
list (APPEND DNF_COMMAND_CLEAN "plugins/clean/dnf-command-clean.c")

include_directories (${CMAKE_CURRENT_SOURCE_DIR})
add_executable (microdnf dnf-main.c ${DNF_SRCS}
                ${DNF_COMMAND_INSTALL}
                ${DNF_COMMAND_REMOVE}
                ${DNF_COMMAND_UPDATE}
                ${DNF_COMMAND_CLEAN})

target_link_libraries (microdnf
                       ${GLIB_LIBRARIES}
                       ${GOBJECT_LIBRARIES}
                       ${PEAS_LIBRARIES}
                       ${LIBDNF_LIBRARIES}
                       ${SCOLS_LIBRARIES})
target_compile_definitions (microdnf
                            PRIVATE -DBUILDDIR="${CMAKE_CURRENT_BINARY_DIR}"
                            PRIVATE -DSRCDIR="${CMAKE_CURRENT_SOURCE_DIR}")

add_subdirectory (plugins)

install (TARGETS microdnf
         RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})
