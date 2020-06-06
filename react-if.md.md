---


---

<h1 id="master-conditional-rendering-in-react-with-if-as-a-component">Master conditional rendering in react with IF as a component</h1>
<p>Hi Readers,  this is my first bite post or any kind of post so please bear with me.</p>
<p>I am working on react since a year approx. And one thing I just didn’t like was the conditional rendering means showing some content based on conditions. Which grows to a nested conditional rendering soon as to fit more business logic.</p>
<p>This is not react-ish way I feel  and mostly it’s a hit more where it hurts, yes readability and maintainability of code. That’s why came of with a simple IF as a component to satisfy basic conditional rendering need.</p>
<p>So below is the code we are writing now, this is a example it goes more that two conditions also most of the time.</p>
<pre><code>render() {
    return (
      { 
	      iftrue ? &lt;DoSomething /&gt; : ifTrueAgain ? &lt;DoSomethingMore /&gt; : null
	  }
}
</code></pre>
<p>This is what we want to write.</p>
<pre><code>&lt;If cond={iftrue}&gt;
    &lt;DoSomething type="pass"/&gt;
    &lt;If type="fail" cond={ifTrueAgain}&gt;
	    &lt;DoSomethingMore type="pass"/&gt;
	    &lt;div type="fail"&gt;Don't do Anything&lt;/div&gt;
    &lt;/If&gt;
&lt;/If&gt;
</code></pre>
<p>Let’s see how we can achieve the above.</p>
<p>My first approach was to just create a higher order function which takes a <strong>cond</strong>  prop and return the children component/JSX if the passed condition is met. i.e:-</p>
<pre><code>const  If  =  props  =&gt;  {
	return props.cond ? props.children : null;  
};

// in component it can used like this 

&lt;If cond={iftrue}&gt;
    {/* Do Something */}
&lt;/If&gt;

// like this

&lt;If cond={iftrue}&gt;
    {/* Do Something */}
    &lt;If cond={iftrueAgain}&gt;
	    {/* Do Something More*/}
    &lt;/If&gt;
&lt;/If&gt;
</code></pre>
<p>This solves our basic problem but then after analysing my personal projects I realised that most of the time there will be an else part (either component/JSX) , not necessarily null every time. So I enhanced the previous example.</p>
<pre><code>const  If  =  props  =&gt;  {
	const { cond } = props;
	return props.children.filter(child =&gt; child.props.type === (cond ? "pass" : "fail");
};

// in component it can used like this 

&lt;If cond={iftrue}&gt;
    &lt;Success type="pass" /&gt;
    &lt;Faliure type="fail" /&gt;
&lt;/If&gt;

// Or like this

&lt;If cond={iftrue}&gt;
    &lt;Success type="pass" /&gt;
    &lt;If type="fail" cond={iftrueAgain}&gt;
	       &lt;SuccessMore type="pass" /&gt;
	       &lt;Faliure type="fail" /&gt;
    &lt;/If&gt;
&lt;/If&gt;
</code></pre>
<p>Here I introduces a prop named <strong>type</strong> to the children components/JSX elements. So if the type is “pass” it will be renderes when the cond is met and the elements with type “fail” will render when the condition will be false.</p>
<blockquote>
<p>If is also treated as a component so that you can write nested conditions as shown in the example.</p>
</blockquote>
<p>One last enhancement was to be done to fix a bug :). When you don’t want to pass a fail/else block.</p>
<pre><code>const  If  =  props  =&gt;  {
	const { cond } = props;
	const isChildrenArray = props.children instanceof Array;
	if() {
	    return props.children.filter(child =&gt; child.props.type === (cond ? "pass" : "fail");
	} else {
		return props.children.props.type === "pass" ? props.children : null;
	}
};
</code></pre>
<p>Please find a live example. looking for a constructive feedback.</p>
<iframe src="https://codesandbox.io/embed/condescending-shannon-tqbjb?fontsize=14&amp;hidenavigation=1&amp;theme=dark" title="condescending-shannon-tqbjb"></iframe>
<p><em>Other edge cases can be found but the main purpose of the post is to demonstrate the idea.</em></p>

