FROM golang:1.21 AS base

# Used for local development
FROM base as local

# Used as builder for deployment
FROM base as base-deploy

# Used for deployment
FROM base-deploy as deploy