# Local-Firebase (modular), your auth-cacher for firebase (in React)!

### https://codesandbox.io/s/react-local-firebase-i7l8qe

LICENSE AGPL-3
No redistribution but for strategy of parts, unless retributed

how to use

````
class Auth extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      auth: undefined,
      user: undefined,
      meAuth: {},
      storableAuth: []
    };
    this.pa = React.createRef();
    this.gui = React.createRef();
    this.ra = React.createRef()
    //this.Vintages = React.createRef();
  }
render() {
    const logoutofapp = async () => {
       var answer = window.confirm("Are you sure you want to log out?");
      if (!answer) return this.ra.current.click();

      await setPersistence(getAuth(), browserSessionPersistence);

      signOut(getAuth())
        .then(() => {
          console.log("logged out");
          this.pa.current.click({}, true);
        })
        .catch((err) => {
          console.log(err);
        });
    };
  return (
    <div>
      <PromptAuth
        ref={{ current: { pa: this.pa, fwd: this.fwd, ra: this.ra } }}
        /*old version
        getRefs={() => {
          return {
            pa: this.pa,
            gui: this.gui
          };
        }}
        reset={this.state.resetAuth}
        resetResetAuth={() => this.setState({ resetAuth: null })}
        */
        
        storableAuth={this.state.storableAuth}
        clearAuth={() => this.setState({ storableAuth: [] })}
        onPromptToLogin={() => this.props.history.push("/login")}
        verbose={true}
        onStart={() => this.props.loadGreenBlue("loading authentication...")}
        setFireAuth={(answer) => {
            if (Object.keys(answer).includes("isStored")) {
              console.log("isStored");
              answer.logout && window.location.reload();
            } else if (Object.keys(answer).includes("meAuth"))
              this.setState(
                {
                  meAuth: answer.meAuth
                },
                () => {
                  if (answer.meAuth.isAnonymous)
                    return console.log("anonymous");
                  //console.log(answer.meAuth);
                  if (!answer.meAuth.uid)
                    //indexOf entry is undefined
                    return this.setState(
                      {
                        user: undefined,
                        auth: undefined,
                        loaded: false
                      }
                      //() => this.addUserDatas(answer.meAuth, user)
                    );
                  onSnapshot(
                    doc(firestore, "users", answer.meAuth.uid),
                    (doc) => {
                      //this.props.unloadGreenBlue();
                      if (doc.exists()) {
                        var user = doc.data();
                        user.id = doc.id;
                        //console.log(user);
                        //console.log(answer.meAuth);
                        this.setState(
                          {
                            user,
                            auth: answer.meAuth,
                            loaded: true
                          }
                          //() => this.addUserDatas(answer.meAuth, user)
                        );
                      }
                    }
                    //standardCatch
                  );
                }
              );
        }}
        onFinish={() => this.props.unloadGreenBlue()}
        meAuth={this.state.meAuth === undefined ? null : this.state.meAuth}
        auth={this.state.auth === undefined ? null : this.state.auth}
      />

      <Data
        meAuth={this.state.meAuth}
        getUserInfo={this.gui.current.click}//this.getUserInfo()}
        saveAuth={(x, hasPermission) => 
          this.setState(
            { storableAuth: [x, true, hasPermission] },
            this.pa.current.click//promtAuth
          )
        }
        logoutofapp={logoutofapp}
````

SEE LICENSE IN LICENSE.lz.txt

`copying the src code? https://github.com/npm/cli/issues/3514
npm install --force && npm uninstall @babel/polyfill --save ...`
