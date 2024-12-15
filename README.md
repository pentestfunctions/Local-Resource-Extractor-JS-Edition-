# Local-Resource-Extractor-JS-Edition-
Extracts Javascript Functions from the current domain


Simply copy the javascript contents of the bookmark as a bookmarklet and bobs your uncle

```js
javascript:(function(){function extractFunctionsFromScript(t){const n=[];const o=/function\s+(\w+)\s*(\([^)]*\))\s*{/g;const r=/const\s+(\w+)\s*=\s*(\([^)]*\))\s*=>\s*{/g;let s;while((s=o.exec(t))!==null){const[e,c,a]=s;n.push({type:"named",name:`${c}${a}`,signature:`function ${c}${a}`})}while((s=r.exec(t))!==null){const[e,c,a]=s;n.push({type:"arrow",name:`${c}${a}`,signature:`const ${c}${a} = () => {`})}return n}function displayFunctions(t){const n=document.createElement("div");n.style.position="fixed";n.style.top="10%";n.style.left="10%";n.style.width="80%";n.style.height="80%";n.style.backgroundColor="white";n.style.border="2px solid black";n.style.zIndex="10000";n.style.padding="20px";n.style.overflowY="scroll";n.style.boxShadow="0 0 10px rgba(0,0,0,0.5)";const o=document.createElement("button");o.textContent="Close";o.style.position="absolute";o.style.top="10px";o.style.right="10px";o.onclick=()=>document.body.removeChild(n);const r=document.createElement("div");r.innerHTML=t.map((t,n)=>`
            <h3>Script: ${t.src || 'Unknown Source'}</h3>
            <pre>${t.functions.map((t,o)=>`${n+1}.${o+1}. ${t.name}`).join("\n")}</pre>
        `).join("<hr>");n.appendChild(o);n.appendChild(r);document.body.appendChild(n)}function collectDomainScripts(){const t=window.location.hostname,n=[];const o=document.getElementsByTagName("script");for(let r of o){const o=r.src;if(!o)continue;try{const r=new URL(o);if(r.hostname!==t)continue}catch{continue}fetch(o).then(t=>t.text()).then(r=>{if(r.includes("Copyright"))return null;const s=extractFunctionsFromScript(r);s.length>0&&n.push({src:o,functions:s}),n.length>0&&displayFunctions(n)}).catch(t=>{console.warn(`Could not fetch ${o}:`,t)})}}collectDomainScripts()})();
```
