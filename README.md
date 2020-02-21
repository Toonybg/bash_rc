# bash_rc
if [ -f ~/.git-completion.bash ]; then
  . ~/.git-completion.bash
fi

# get current branch in git repo
function parse_git_branch() {
	BRANCH=`git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/\1/'`
	if [ ! "${BRANCH}" == "" ]
	then
		STAT=`parse_git_dirty`
		echo "[${BRANCH}${STAT}]"
	else
		echo ""
	fi
}

# get current status of git repo
# get current branch in git repo
function parse_git_branch() {
	BRANCH=`git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/\1/'`
	if [ ! "${BRANCH}" == "" ]
	then
		STAT=`parse_git_dirty`
		echo "[${BRANCH}${STAT}]"
	else
		echo ""
	fi
}

# get current status of git repo
function parse_git_dirty {
	status=`git status 2>&1 | tee`
	dirty=`echo -n "${status}" 2> /dev/null | grep "modified:" &> /dev/null; echo "$?"`
	untracked=`echo -n "${status}" 2> /dev/null | grep "Untracked files" &> /dev/null; echo "$?"`
	ahead=`echo -n "${status}" 2> /dev/null | grep "Your branch is ahead of" &> /dev/null; echo "$?"`
	newfile=`echo -n "${status}" 2> /dev/null | grep "new file:" &> /dev/null; echo "$?"`
	renamed=`echo -n "${status}" 2> /dev/null | grep "renamed:" &> /dev/null; echo "$?"`
	deleted=`echo -n "${status}" 2> /dev/null | grep "deleted:" &> /dev/null; echo "$?"`
	bits=''
	if [ "${renamed}" == "0" ]; then
		bits=">${bits}"
	fi
	if [ "${ahead}" == "0" ]; then
		bits="*${bits}"
	fi
	if [ "${newfile}" == "0" ]; then
		bits="+${bits}"
	fi
	if [ "${untracked}" == "0" ]; then
		bits="?${bits}"
	fi
	if [ "${deleted}" == "0" ]; then
		bits="x${bits}"
	fi
	if [ "${dirty}" == "0" ]; then
		bits="!${bits}"
	fi
	if [ ! "${bits}" == "" ]; then
		echo " ${bits}"
	else
		echo ""
	fi
}
#with path
export PS1="[\[\e[32m\]\t\[\e[m\]]\w\`parse_git_branch\`\[\e[32m\]\\$\[\e[m\]>"
#with current dir only


change_terminal_var=0
change_terminal_prompt() {  
  if [ $change_terminal_var -eq 0 ]
  then 
    export PS1="[\[\e[32m\]\t\[\e[m\]]\W\`parse_git_branch\`\[\e[32m\]\\$\[\e[m\]>"
    change_terminal_var=1
  else
    export PS1="[\[\e[32m\]\t\[\e[m\]]\w\`parse_git_branch\`\[\e[32m\]\\$\[\e[m\]>"
    change_terminal_var=0
  fi
}

alias ctp=change_terminal_prompt
#creating folder and opening
alias ..='cd ..'
alias ...'=cd ../..'
alias ....='cd ../../..'

alias ll='ls -l'

alias resource='source ~/.bash_profile'

mkcd () { mkdir -p "$1" && cd "$1"; }

alias rmdir='rm -rf'
 
alias d='date'
alias ff='find . -name $1'
alias h='history | tail'
