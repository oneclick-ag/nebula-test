#!/usr/bin/env bash
################################################################################
# Use Bash Strict Mode
set -o errexit
set -o nounset
set -o pipefail
#set -o errtrace
#set -o xtrace
IFS=$'\n\t'
################################################################################
function check_term() {
   TERM=${TERM:-}
   if [[ -z "${TERM}" ]]; then
      printf "%s\n" "TERM - ${TERM}"
      export TERM=xterm-256color
      printf "%s\n" "TERM - ${TERM}"
   elif [[ -n "${TERM}" ]]; then
      if [[ "${TERM}" == 'dumb' ]]; then
         printf "%s\n" "TERM - ${TERM}"
         export TERM=xterm-256color
         printf "%s\n" "TERM - ${TERM}"
      fi
   fi
}

function colors_vars() {
   R_C=$(tput setaf 1)
   G_C=$(tput setaf 2)
   W_C=$(tput setaf 7)
   #BRIGHT=$(tput bold)
   N_C=$(tput sgr0)
}

function print_f() {
   printf "%s\n" "${N_C}${W_C}($0) ${N_C}${1}${N_C}"
}

function readlinkf() {
   perl -MCwd -e 'print Cwd::abs_path shift' "${1}"
}

function check_if_macos() {
   if [ "$(uname)" != Darwin ]; then
      print_f "${R_C}FAILED - Please run this script on ${Y_C}macOS${R_C}!"
      exit 1
   fi
}
################################################################################
SCRIPT_ROOT_DIR=$(
   cd "$(dirname "$(readlinkf "${BASH_SOURCE[@]}")")"
   pwd
)
SRC_DIR=.oneclick-mesh-client
OC_LOGO_ASCII=oneclick-logo-ascii.txt
OC_LOGO_ASCII_SRC="${SCRIPT_ROOT_DIR}/${SRC_DIR}/${OC_LOGO_ASCII}"
ST_SCRIPT="start.sh"
ST_SCRIPT_SRC="${SCRIPT_ROOT_DIR}/${SRC_DIR}/${ST_SCRIPT}"
THIS_SCRIPT_NAME=$(basename "$0")
################################################################################

function get_lang() {
   LCL_LANG=$(
      osascript -e \
         'set lang to first word of (do shell script "defaults read NSGlobalDomain AppleLanguages")'
   )
   if [[ "${LCL_LANG}" == "en" || "${LCL_LANG}" != "de" ]]; then
      PROMPT_MSG="Oneclick Mesh Client install..."
   elif [[ "${LCL_LANG}" == "de" ]]; then
      PROMPT_MSG="Oneclick Mesh Client installieren..."
   fi
}

function show_logo() {
   cat "${OC_LOGO_ASCII_SRC}"
}

function main() {
   get_lang
   clear
   show_logo
   printf "%s\n" "${N_C}${G_C}${PROMPT_MSG}${N_C}"
   set +o errexit
   osascript \
      -e "do shell script \"/usr/bin/env bash ${ST_SCRIPT_SRC} && exit\" \
      with prompt \"${PROMPT_MSG}\" with administrator privileges"
   set -o errexit
   osascript \
      -e "tell application \"Terminal\" to close (every window whose name \
      contains \"${THIS_SCRIPT_NAME}\")" &
   exit
}

# Main #########################################################################
check_term
colors_vars
check_if_macos
main
