# Deploy the MutationWebhook on OpenShift


1. Update the `CA_BUNDLE` for the webhook

    ~~~sh
    deploy/updatecabundle.sh deploy/mutatingwebhook.yaml
    ~~~

2. Deploy the `MutatingWebhookConfiguration`:

    ~~~sh
    oc create -f deploy/mutatingwebhook.yaml
    ~~~

3. Deploy the webhook service

    ~~~sh
    oc create -f deploy/webhook-svc-deployment.yaml
    ~~~

4. Test the webhooks:

    1. Mutation Webhook:

        > **NOTE**: We need to tag the namespace where we want the webhook to mutate the pull policy

        ~~~sh
        oc label namespace openshift-marketplace pullpolicymutator=yes
        ~~~

        > **NOTE**: Delete the pods, so they're recreated and the pullpolicy gets updated

        ~~~sh
        oc -n openshift-marketplace delete pods --all
        ~~~

5. Clean everything:

    ~~~sh
    oc delete -f deploy/webhook-svc-deployment.yaml 
    ~~~

    ~~~sh
    oc delete -f deploy/mutatingwebhook.yaml
    ~~~