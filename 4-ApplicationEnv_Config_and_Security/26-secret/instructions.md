# Exercise 26

<details>
<summary><b>Quick Reference</b></summary>
<p>

* Namespace: `default`<br>
* Documentation: [Secrets](https://kubernetes.io/docs/concepts/configuration/secret/)

</p>
</details>

In this exercise, you will first create a Secret from literal values. Next, you'll create a Pod and consume the Secret as environment variables. Finally, you'll print out its values from within the container.



1. Create a new Secret named `db-credentials` with the key/value pair `db-password=passwd`.
2. Create a Pod named `backend` that uses the Secret as environment variable named `DB_PASSWORD` and runs the container with the image `nginx:1.23.4-alpine`.
3. Shell into the Pod and print out the created environment variables. You should be able to find the `DB_PASSWORD` variable.
4. (Optional) Discuss: What is one of the benefit of using a Secret over a ConfigMap?
