# Git Repository

**Git repositories** are powerful resources for passive reconnaissance, enabling you to search through codebases and commit histories for exposed secrets and sensitive information.&#x20;

The following examples shows how it is possible to focus the search by specifying:

* **`owner`:** Identifies the repository owner.
* **`url`:** Points to the specific file URL.

```
owner:<repository_owner> path:<file_path>
```

To automate this process and search inside commits history, the following tools are commonly used:

{% embed url="https://github.com/michenriksen/gitrob" %}
gitrob tool
{% endembed %}

{% embed url="https://github.com/gitleaks/gitleaks" %}
gitleaks tool
{% endembed %}
