```js
import React from "react";
import {
    BrowserRouter as Router,
    Switch,
    Route,
    Link,
    Redirect,
    useHistory,
    useLocation
} from "react-router-dom";

// 这个例子有3个页面: 一个公共的, 一个受保护的
// 页面, 和一个登陆屏幕. 为了看到受保护的页面, 你必须第一次使用. 相当标准的东西.
// 首先, 访问 公共页面. 然后, 访问protected页面 .你没有登陆的话, 所以你被重定向到login页面.登录后, 你被重定向返回到受保护的页面.
// 注意URL每次都会变.在此刻如果你点击返回按钮, 你是否希望回到登录页面? 不! 你已经登陆了. 试试看,
// 您会看到您返回到您的访问页面仅在登陆之前的页面。


export default function AuthExample() {
    return (
        <Router>
            <div>
                <AuthButton />

                <ul>
                    <li>
                        <Link to="/public">Public Page</Link>
                    </li>
                    <li>
                        <Link to="/protected">Protected Page</Link>
                    </li>
                </ul>

                <Switch>
                    <Route path="/public">
                        <PublicPage />
                    </Route>
                    <Route path="/login">
                        <LoginPage />
                    </Route>
                    <PrivateRoute path="/protected">
                        <ProtectedPage />
                    </PrivateRoute>
                </Switch>
            </div>
        </Router>
    );
}

const fakeAuth = {
    isAuthenticated: false,
    authenticate(cb) {
        fakeAuth.isAuthenticated = true;
        setTimeout(cb, 100); // fake async
    },
    signout(cb) {
        fakeAuth.isAuthenticated = false;
        setTimeout(cb, 100);
    }
};

function AuthButton() {
    let history = useHistory();

    return fakeAuth.isAuthenticated ? (
        <p>
            Welcome!{" "}
            <button
                onClick={() => {
                    fakeAuth.signout(() => history.push("/"));
                }}
            >
                Sign out
            </button>
        </p>
    ) : (
        <p>You are not logged in.</p>
    );
}

//<Route>的包装器，重定向到登录屏幕，如果您尚未通过身份验证。
function PrivateRoute({ children, ...rest }) {
    return (
        <Route
            {...rest}
            render={({ location }) =>
                fakeAuth.isAuthenticated ? (
                    children
                ) : (
                    <Redirect
                        to={{
                            pathname: "/login",
                            state: { from: location }
                        }}
                    />
                )
            }
        />
    );
}

function PublicPage() {
    return <h3>Public</h3>;
}

function ProtectedPage() {
    return <h3>Protected</h3>;
}

function LoginPage() {
    let history = useHistory();
    let location = useLocation();       //useLocation挂钩返回代表当前URL的位置对象。


    let { from } = location.state || { from: { pathname: "/" } };
    let login = () => {
        fakeAuth.authenticate(() => {
            history.replace(from);
        });
    };

    return (
        <div>
            <p>You must log in to view the page at {from.pathname}</p>
            <button onClick={login}>Log in</button>
        </div>
    );
}

```

这可能非常有用，例如在您希望每次加载新页面时都使用Web分析工具触发新的“页面浏览”事件的情况下，如以下示例所示：
